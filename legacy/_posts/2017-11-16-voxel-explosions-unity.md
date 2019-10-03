---
id: 507
title: Voxel Explosions in Unity
date: 2017-11-16T18:00:04+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=507
permalink: /2017/11/voxel-explosions-unity/
categories:
  - game-development
tags:
  - dotnet
  - csharp
  - development
  - gamedev
  - programming
  - unity
---
This article describes the process of creating voxel explosions in Unity by using particles. For our models, we use [MagicaVoxel](https://ephtracy.github.io/), but every other tool can be used as well. The final result will look similar to this:

<iframe width="560" height="315" src="https://www.youtube.com/embed/n4mzQTFuP68?rel=0&amp;controls=0&amp;showinfo=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Setting up the test environment

For test purposes, I&#8217;ve set up a basic FPS environment using the built-in CharacterController package. I then added a gun model to the _FirstPersonCharacter _game object, which is located as child element in the hierarchy of the _FPSController_. The following snippet shows the code I added as a custom script component to the gun:

{% highlight c# %}
using UnityEngine;

public class GunScript : MonoBehaviour {

    public Camera FPSCamera;
    public float maxDistance = 100f;

    public AudioClip shot;
    float fireRate = 0.5f;
    private float nextFire = 0.0f;
    private Transform t;

    void Start () {
        t = transform;
    }

    void Update ()
    {
        if(Input.GetButtonDown("Fire1") && IsOkToShoot())
        {
            Shoot();
        }
    }

    void Shoot()
    {
        AudioSource.PlayClipAtPoint(shot, t.position);
        RaycastHit rcHit;
        if(Physics.Raycast(FPSCamera.transform.position, FPSCamera.transform.forward, out rcHit, maxDistance))
        {
            if(rcHit.transform.tag == "Enemy")
            {
                ExplosionScript ex = rcHit.transform.gameObject.GetComponent<ExplosionScript>();
                
                if(ex != null)
                {
                    ex.Explode(rcHit.point);
                }
            }
        }
    }

    bool IsOkToShoot()
    {
        bool fireable = false;
        if (Time.time > nextFire) {
            nextFire = Time.time + fireRate;
            fireable = true;
        }
        return fireable;
    }
}{% endhighlight %}

So basically, this script takes up three public variables, one for the camera of the controller, which is needed to shoot from the player&#8217;s perspective, a maxDistance to set how far can be shot, and an AudioClip to play a sound file when a shot has been fired. Additionally, there are a few private variables:

  * _fireRate,_ to set how fast shots can be fired after each other
  * _nextFire_, which is used to determine when a shot can be fired
  * _t_, which represents the transform component of the current gameObject

Every frame I check if the left mouse button is pressed. If so, a shot should be fired if the method _IsOkToShoot()_ returns _true_. This happens when the current time minus the last time shot is larger than the previously set _fireRate_.

When an actual shot happens, the sound is being played where _t.position_ determines the location from where the shot is going to be heard. After that, I call up a ray cast to scan the targeted environment for possible targets. The outgoing argument _hit_ provides information about the target that has been hit (if nothing has been hit, the code inside the outermost if-statement will not be executed).

I created a custom tag called _Enemy_, which is going to be added to all the objects I want to hit. These are going to get a custom script called _ExplosionScript_, which contains a method named _Explode_. The next step is to build this script and actually blow something up.

## Exploding Voxels

I&#8217;m using a basic model for testing purposes, that can bee seen in the screenshot below.

![Voxel Explosion - Unity Prefab](/assets/2017/voxel_explosion_1.png)

It currently consists of an empty _GameObject_ as parent, which has been expaned by adding a _MeshCollider_. The child element contains the actual content.

At first, a _ParticleSystem_ is going to be added to the object. I use the following configuration here:

![Voxel Explosion - Unity Prefab Settings](/assets/2017/voxel_explosion_2.png)

Note that the last setting (shader) can&#8217;t be set yet. That will be created later. Also, _ExplosionMat_ is just a regular material. After configuring the _ParticleSystem_, the animation should look like some cubes exploding and falling on the ground. Now the colors of the cubes need to be adjusted. The problem is, that a custom shader and a bit of additional coding is needed here. Now you just create a new prefab and move the _ParticleSystem_ onto it. Then delete it from the object that has been used for testing.

I&#8217;ll start with the shader. You use the context menu and add a new _shader_ &#8211; _standard surface shader_. You can open the newly created file with Visual Studio (or whatever editor you want) and edit it to match our plan:

{% highlight c# %}Shader "Custom/ExplosionShader" {
    Properties {
        _Color ("Color", Color) = (1,1,1,1)
        _MainTex ("Albedo (RGB)", 2D) = "white" {}
        _Glossiness ("Smoothness", Range(0,1)) = 0.5
        _Metallic ("Metallic", Range(0,1)) = 0.0
    }
    SubShader {
        Tags { "RenderType"="Opaque" }
        LOD 200
        
        CGPROGRAM
        // Physically based Standard lighting model, and enable shadows on all light types
        #pragma surface surf Standard fullforwardshadows

        // Use shader model 3.0 target, to get nicer looking lighting
        #pragma target 3.0

        sampler2D _MainTex;

        struct Input {
            float2 uv_MainTex;
            
            // NEW LINE
            float4 vertexColor : COLOR;
        };

        half _Glossiness;
        half _Metallic;
        fixed4 _Color;

        void surf (Input IN, inout SurfaceOutputStandard o) {
            // Albedo comes from a texture tinted by color
            fixed4 c = tex2D (_MainTex, IN.uv_MainTex) * _Color;
            
            // NEW LINE:
            c *= IN.vertexColor;


            o.Albedo = c.rgb;
            // Metallic and smoothness come from slider variables
            o.Metallic = _Metallic;
            o.Smoothness = _Glossiness;
            o.Alpha = c.a;
        }
        ENDCG
    }
    FallBack "Diffuse"
}{% endhighlight %}

There are two lines that need to be adjusted, I&#8217;ve marked them with the comment _//NEW LINE_. After that, the currently used material for the particles should be set to the color white. The color that is going to be set later will be mixed with the default color, and if that isn&#8217;t white the colors won&#8217;t match our wanted ones. Also, the shader of the material needs to be set to the newly created one.

An additional script is needed to edit the palette.

{% highlight c# %}using UnityEngine;

public class ExplosionScript : MonoBehaviour {

    public GameObject explosionPrefab;
    private GameObject explosion;
    bool exploding = false;

    public string colorDescriptor;
    private Color32[] colors;

    public void Explode(Vector3 pos)
    {
        explosion = Instantiate(explosionPrefab, pos, Quaternion.Euler(0f,0f,0f));
        exploding = true;       
    }

    private void LateUpdate()
    {
        if(exploding)
        {
            ParticleSystem explosionParticleSystem = explosion.GetComponent<ParticleSystem>();
            ParticleSystem.Particle[] particles = new ParticleSystem.Particle[explosionParticleSystem.main.maxParticles];
            colors = ColorManager.getColor(colorDescriptor);
            int numParticlesAlive = explosionParticleSystem.GetParticles(particles);
            for (int i = 0; i < numParticlesAlive; i++)
            {
                particles[i].startColor = colors[Random.Range(0, colors.Length)];
            }
            explosionParticleSystem.SetParticles(particles, numParticlesAlive);
            exploding = false;

            Destroy(gameObject);
        }       
    }
}{% endhighlight %}

Again, here are some public and private variables:

  * _explosionPrefab_ represents the prefab containing the ParticleSystem
  * _explosion_ is an instance of the prefab
  * _exploding_ is a flag for controlling the explosion
  * _colors_ is an array containing the colors that are going to be assigned to the exploding particles

The first method starts the explosion. It instantiates the explosion at a given position, which is going to be where the ray cast hit the object. In _LateUpdate()_, the particles are going to be painted. This method is only run, when the exploding-flag is set to _true_, and it is also run only once, which is why the flag is set to _false_ again at the end.

The _gameObject_ itself is destroyed in the very end, since it is needed to assign its properties to the _ParticleSystem_ created at runtime.

All the particles of the explosion are collected inside the _particles _array. The for-loop is iterating through all of the particles and assigns a new color. The color is determined via random selection of a color of the above-mentioned array. I found this solution to be a bit ugly, so I created another script called _ColorManager_, which contains arrays for each of my models. It basically just contains the following code:

{% highlight c# %}private static Color32[] foxColors =
{
    new Color32(238, 0, 0, 255),
    new Color32(0, 0, 34, 255),
    new Color32(238,238,238,255),
    new Color32(187,187,187,255)
};

private static Color32[] chickenColors =
{
    new Color32(238,238,238,255),
    new Color32(238,0,0,255),
    new Color32(255,102,0,255),
    new Color32(0,0,17,255)
};

private static Color32[] emptyColors =
{
    new Color32(255,255,255,255)
};

public static Color32[] getColor(string colorDescription)
{
    switch(colorDescription)
    {
        case "fox":
            return foxColors;
        case "chicken":
            return chickenColors;
        default:
            return emptyColors;
    }
}{% endhighlight %}

&nbsp;

Note that you can view the colors that have been used using MagicaVoxel by clicking on the tool marked by a _smaller-than sign_ and then clicking on _HSV_.

![Voxel Explosion - MagicaVoxel Color Palette](/assets/2017/voxel_explosion_3.png)

If this is done, the explosions should work!