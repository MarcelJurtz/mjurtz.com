---
id: 485
title: Building a Chess Game in Unity
date: 2017-11-13T17:57:49+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=485
permalink: /2017/11/building-a-chess-game-in-unity/
categories:
  - CSharp
  - Game development
tags:
  - Development
  - GameDev
  - Programming
  - Unity
---
In this post, I&#8217;ll be covering the basics of building a simple chess game in Unity and C#.

The MVP of this project will be a fully playable 3D chess game for two players. Each character will only be able to make moves that comply with the rules. After slaying a king, the game is reset. To improve the game, menu scenes, statistics and a simple AI will be added later. The final project and its source code is available on my [GitHub-profile](https://github.com/MarcelJurtz/Chess).

Since this article became quite large, I split it into different parts. Use one of the following links to jump directly to the relevant section.

  1. [Loading the Assets](#loading_assets) for programmers lacking 3d-modeling skills
  2. [Setting up the Scene](#setting_up_scene) to create a basic setting
  3. [Drawing the Chessboard](#drawing_chessboard) to make testing easier
  4. [Initializing the Characters](#initialize_characters) and move them to their default location
  5. [Movement Control](#movement_control) to differentiate between selecting and moving characters
  6. [Board Highlighting](#board_highlighting) to mark all fields to which can be moved
  7. [Obey the Rules](#obey_rules) to implement each characters movesets

## Loading the Assets {#loading_assets}

I think it is quite common that programmers are bad designers. At least that&#8217;s the case for me, which is why I try to get the graphic elements for my chess game from the Asset Store. I found [Arcane Cyber&#8217;s Classic Chess Set](https://www.assetstore.unity3d.com/en/#!/content/93014) to look pretty nice and decided to give it a shot. This asset pack contains three different types of figures (metallic, marble & plastic), although the board is only available in the marble look.

## Setting up the Scene {#setting_up_scene}

I start with creating a simple scene, which contains a camera and a light source, as well as an empty GameObject for the chessboard. The chessboards prefab is attached to this gameobject as a child element. Another child element is a simple plane, which I use for testing. For the time being, I deactivate the chess board itself to get a better overview.

As soon as the scene itself is finished, I start with the actual scripting. The parent element of the chess field receives a script called BoardManager, which manages the whole game.

## Drawing the Chessboard {#drawing_chessboard}

The BoardManager has several tasks, which I&#8217;ll be covering in the following. First of all, the manager loads all the characters at their starting positions. Then, possible movements are calculated and the observance of the alternating moves is checked.

I&#8217;ll start by drawing a chessboard manually. This helps to test the correct movements and positioning inside the squares of the chess board. The method is called in the scripts Start()-method.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void DrawChessBoard()
{
    Vector3 widthLine = Vector3.right * 8;
    Vector3 heightLine = Vector3.forward * 8;

    for (int i = 0; i &lt;= 8; i++)
    {
        Vector3 start = Vector3.forward * i;
        Debug.DrawLine(start, start + widthLine);
        for (int j = 0; j &lt;= 8; j++)
        {
            start = Vector3.right * j;
            Debug.DrawLine(start, start + heightLine);
        }
    }
}</pre>

This function generates an 8&#215;8 grid, which symbolizes the chess board and is now used to simplify the positioning of the chess pieces. Before I will position the game pieces next, I want to add a function to hold the current mouse position on the chess board. This function will be implemented by displaying a cross on the labelled field.

The currently selected position will be identified by two integer values, which represent X and Y position, ranging from 0 to 7. These two variables are defined at class level and have a default value of -1, which indicates that no field on the chessboard is selected. This value should also be assumed if the mouse position is outside of the chess field. To update the selection, I move the call of _DrawChessboard()_ from the _Start()_&#8211; to the _Update()_-method.

I use a Raycast to check if the mouse is inside the board. I also assign the chessboard GameObject its own layer called &#8220;ChessPlane&#8221;, which is checked for availability by the Raycast.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void UpdateSelection()
{
    if (!Camera.main) return;

    RaycastHit hit;
    float raycastDistance = 25.0f;
    if (Physics.Raycast(Camera.main.ScreenPointToRay(Input.mousePosition), out hit, raycastDistance, LayerMask.GetMask("ChessPlane")))
    {
        selectionX = (int)hit.point.x;
        selectionY = (int)hit.point.z;
    }
    else
    {
        selectionX = -1;
        selectionY = -1;
    }
}</pre>

This method is also called in the _Update()_-method. As you can see in the snippet above, only the X and Y variables are set in the method, but no corresponding changes are made to the graphical user interface. I implement this functionality in the _DrawChessBoard()_-method to maintain a clear code structure through the related functional area. So, after drawing the board itself, I use the following code to display the current mouse position. By this instruction, two further lines are drawn, which are represented in the form of a cross on the chess board.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">if (selectionX &gt;= 0 && selectionY &gt;= 0)
{
    Debug.DrawLine(Vector3.forward * selectionY + Vector3.right * selectionX,
        Vector3.forward * (selectionY + 1) + Vector3.right * (selectionX + 1));
    Debug.DrawLine(Vector3.forward * selectionY + Vector3.right * (selectionX + 1),
        Vector3.forward * (selectionY + 1) + Vector3.right * selectionX);
}</pre>

## Initializing the Characters {#initialize_characters}

Next, the chess pieces are initialized and positioned at their respective locations. For this purpose I need information about the different game pieces, so a prefab for each piece has to be created. The package from the Asset Store I used already has prebuilt assets, but you&#8217;ll want to adjust the scaling and rotation.

I would like to create a class for each type of character so that I can deduce the different possible movements later on. But this is not needed at the moment, so I create a superclass which describes each character and is later used as a parent class of the different characters.

I&#8217;ve already done some preliminary work here, which is why this class needs an explanation. Each figure has a position, described by two-dimensional coordinates (X and Y). In addition, each figure is either white or black and should have information about the positions to which it may move. The differentiation between white and black figures is realized by a boolean. Alternatively, an enumeration could be used here as well, which I think would be better with regard to readability, but the boolean offers the advantage of comparing this property with a corresponding property of the _BoardManager_ to check the current active color and switching it with the != operator. I have provided the class with the keyword _abstract_, which prevents the use of the class itself, so only the subclasses can be used.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public abstract class ChessFigure : MonoBehaviour
{
    public int CurrentX { get; set; }
    public int CurrentY { get; set; }
    public bool isWhite;

    public void SetPosition(int x, int y)
    {
        CurrentX = x;
        CurrentY = y;
    }

    public virtual bool[,] PossibleMove()
    {
        return new bool[8,8];
    }
}</pre>

So much for it. Next, a class is created for each type of character, inherited from the superclass. Since these classes will only overwrite the possible movements, they do not contain any logic of their own at the moment. Remember to adjust the different prefabs after adding the script using the public property &#8220;isWhite&#8221; and set the check mark in the editor to the white figures.

To assign the different prefabs to BoardManager, I&#8217;ll now add a public list of the GameObject type and add the different prefabs to it in the editor. The order of the instances is not important, but I recommend that you build a basic logic to make the instantiation as easy to read as possible later on. In my case, I use the following order:

  1. King (White)
  2. Queen (White)
  3. Rook (White)
  4. Bishop (White)
  5. Knight (White)
  6. Pawn (White)
  7. King (Black)
  8. Queen (Black)
  9. Rook (Black)
 10. Bishop (Black)
 11. Knight (Black)
 12. Pawn (Black)

To instantiate the different figures correctly, different functionalities are needed. The figure must start on the correct field, but also take the right position (centered) within the field. To calculate the center of a field, I created a method GetTileCenter (). This requires the information about the size of a field, which I have stored in a constant called TILE\_SIZE (which is equal to 1). I also calculate the center using an offset (TILE\_OFFSET) of 0.5. The parameters correspond to the coordinates of the respective figure on the chess board, which are represented by X and Y (0-7).

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private Vector3 GetTileCenter(int x, int y)
{
    Vector3 origin = Vector3.zero;
    origin.x += (TILE_SIZE * x) + TILE_OFFSET;
    origin.z += (TILE_SIZE * y) + TILE_OFFSET;
    return origin;
}</pre>

This method is now called by another one, which is used to instantiate the figures. Here too, I have already implemented another functionality, which I must briefly explain. _ChessFigurePositions_ is a two-dimensional array of the type GameObject, which holds information about the positions of figures on the board. When an entry of this is equal to null, that means that no figure is located on that position. _ActiveFigures_ is list of the type GameObject, used to keep track of all characters that are currently alive.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void SpawnChessFigure(int index, int x, int y)
{
    GameObject go = Instantiate(chessFigures[index], GetTileCenter(x, y), chessFigures[index].transform.rotation) as GameObject;
    go.transform.SetParent(transform);
    ChessFigurePositions[x, y] = go.GetComponent&lt;ChessFigure&gt;();
    ChessFigurePositions[x, y].SetPosition(x, y);
    activeFigures.Add(go);
}</pre>

With this method, it is now possible to instantiate game pieces, but there is no information on which position belongs to which figure. I create another method called _SpawnAllChessFigures()_, which handles this task. This is then called in the _Start()_ method of the script and will later also be used to implement the functionality to reset the game. The only difficulty here lies in the correct positioning of the game pieces, which is determined by the parameters of the spawn function.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void SpawnAllChessFigures()
{
    // White
    SpawnChessFigure(0, 4, 0); // King
    SpawnChessFigure(1, 3, 0); // Queen
    SpawnChessFigure(2, 0, 0); // Rook
    SpawnChessFigure(2, 7, 0); // Rook
    SpawnChessFigure(3, 2, 0); // Bishop
    SpawnChessFigure(3, 5, 0); // Bishop
    SpawnChessFigure(4, 1, 0); // Knight
    SpawnChessFigure(4, 6, 0); // Knight
    SpawnChessFigure(5, 0, 1);
    SpawnChessFigure(5, 1, 1);
    SpawnChessFigure(5, 2, 1);
    SpawnChessFigure(5, 3, 1);
    SpawnChessFigure(5, 4, 1);
    SpawnChessFigure(5, 5, 1);
    SpawnChessFigure(5, 6, 1);
    SpawnChessFigure(5, 7, 1);

    // Black
    SpawnChessFigure(6, 4, 7); // King
    SpawnChessFigure(7, 3, 7); // Queen
    SpawnChessFigure(8, 0, 7); // Rook
    SpawnChessFigure(8, 7, 7); // Rook
    SpawnChessFigure(9, 2, 7); // Bishop
    SpawnChessFigure(9, 5, 7); // Bishop
    SpawnChessFigure(10, 1, 7); // Knight
    SpawnChessFigure(10, 6, 7); // Knight
    SpawnChessFigure(11, 0, 6);
    SpawnChessFigure(11, 1, 6);
    SpawnChessFigure(11, 2, 6);
    SpawnChessFigure(11, 3, 6);
    SpawnChessFigure(11, 4, 6);
    SpawnChessFigure(11, 5, 6);
    SpawnChessFigure(11, 6, 6);
    SpawnChessFigure(11, 7, 6);
}</pre>

## Movement Control {#movement_control}

After all characters have been added to the board, they need to be able to change their position. I splt this functionality into two parts. First of all, it must be possible to select a character by clicking on it. Afterwards, the selected character must be able to move to another location (considering its allowed moves) as far as it is the figures colors turn.

To implement this functionality, I have created two new methods in the _BoardManager_: _SelectChessFigure()_ and _MoveChessFigure()_. To keep track of the currently active color, I have created a class wide boolean-variable named _isWhiteTurn_, which can be compared to the _isWhite_ property of the individual characters. The call of these two functions is in the _Update()_-method and listens for a mouse click.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">if(Input.GetMouseButtonDown(0))
{
    if(selectionX &gt;= 0 && selectionY &gt;= 0)
    {
        if(selectedFigure == null) SelectChessFigure(selectionX, selectionY);
        else MoveChessFigure(selectionX, selectionY);
    }
}</pre>

I have created another boolean-variable inside the method, which is used to monitor whether the selected figure can move at all. If this is not the case, the figure should not be selectable, so that a different figure can be selected directly afterwards. Otherwise, the selection of a field to which the piece should moved follows. In case of no other possible movement a double click on a new figure would be required, which is circumvented by this implementation. So, the task of the nested for-loop is simply to check if the chosen figure is able to move.

If the figure is able to move, all available target fields should be highlighted. I have outsourced this functionality to another class, which I will discuss next. The array containing information on the available movements is also filled here. This calculation takes place in the different classes of the individual game pieces, which will also be discussed soon.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void SelectChessFigure(int x, int y)
{
    if (ChessFigurePositions[x, y] == null) return;
    if (ChessFigurePositions[x, y].isWhite != isWhiteTurn) return;

    bool hasAtLeastOneMove = false;
    allowedMoves = ChessFigurePositions[x, y].PossibleMove();

    for(int i = 0; i &lt; 8; i++)
    {
        for(int j = 0; j &lt; 8; j++)
        {
            if(allowedMoves[i,j])
            {
                hasAtLeastOneMove = true;
                i = 7;
                break;
            }
        }
    }

    if (!hasAtLeastOneMove) return;

    selectedFigure = ChessFigurePositions[x, y];
    BoardHighlighting.Instance.HighlightAllowedMoves(allowedMoves);
}</pre>

The _MoveChessFigure()_-Method is implemented quite simple. If there is already a different colored figure on the target field, it will be destroyed. If this figure happens to be a king, the _EndGame-()_-method is called, which resets the game. Afterwards, the board layout and position properties of the selected piece are updated.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void MoveChessFigure(int x, int y)
{
    if(allowedMoves[x,y])
    {
        ChessFigure c = ChessFigurePositions[x, y];
        if(c != null && c.isWhite != isWhiteTurn)
        {
            activeFigures.Remove(c.gameObject);
            Destroy(c.gameObject);

            if(c.GetType() == typeof(King))
            {
                EndGame();
                return;
            }
        }

        ChessFigurePositions[selectedFigure.CurrentX, selectedFigure.CurrentY] = null;
        selectedFigure.transform.position = GetTileCenter(x, y);
        selectedFigure.SetPosition(x, y);
        ChessFigurePositions[x, y] = selectedFigure;
        isWhiteTurn = !isWhiteTurn;
    }

    BoardHighlighting.Instance.HideHighlights();
    selectedFigure = null;
}</pre>

Another detail that is visible in the implementation of the _EndGame()_-method is the access to the _BoardHighlighting_-class. This was implemented as a singleton, whereby only one instance of the class can exist, which is accessible from every area of the game.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">private void EndGame()
{
    if (isWhiteTurn)
        Debug.Log("White team won!");
    else
        Debug.Log("Black team won!");

    foreach (GameObject go in activeFigures)
        Destroy(go);

    isWhiteTurn = true;
    BoardHighlighting.Instance.HideHighlights();
    SpawnAllChessFigures();
}</pre>

## Board Highlighting {#board_highlighting}

The _BoardHighlighting_-class is used to <span>emphasise </span>possible target positions. I use a simple prefab in the form of a square plane, which is instantiated on the potential target fields. The possible fields are set by a parameter in form of a two-dimensional boolean array. The highlight objects are additionally managed in a list, which makes it easy to perform a reset between moves.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class BoardHighlighting : MonoBehaviour {

    public static BoardHighlighting Instance { get; set; }

    public GameObject highlightPrefab;
    private List&lt;GameObject&gt; highlights;

    private void Start()
    {
        Instance = this;
        highlights = new List&lt;GameObject&gt;();
    }

    private GameObject GetHighlightObject()
    {
        GameObject go = highlights.Find(g =&gt; !g.activeSelf);
        if(go == null)
        {
            go = Instantiate(highlightPrefab);
            highlights.Add(go);
        }
        return go;
    }

    public void HighlightAllowedMoves(bool[,] moves)
    {
        for (int i = 0; i &lt; 8; i++)
        {
            for (int j = 0; j &lt; 8; j++)
            {
                if (moves[i, j])
                {
                    GameObject go = GetHighlightObject();
                    go.SetActive(true);
                    go.transform.position = new Vector3(i + 0.5f, 0, j + 0.5f);
                }
            }
        }
    }

    public void HideHighlights()
    {
        foreach (GameObject go in highlights) go.SetActive(false);
    }
}</pre>

And that&#8217;s it! Almost. The only thing missing is the assignment of the possible moves to the different pieces, which will be covered next.

## Obeying the Rules {#obey_rules}

The simplest character is also the most difficult to implement, so I&#8217;ll start with this one. Generally, the pawn can only move forward one field per turn. However, there are special regulations: if the pawn is located on its start field, then he&#8217;s allowed to move two fields forward. He can also not move forward if there is another character on that field. Finally, the pawn may only attack diagonally.

Because the direction in which the figures move is different, I implement this logic for both colors separately:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class Pawn : ChessFigure
{
    public override bool[,] PossibleMove()
    {
        bool[,] r = new bool[8, 8];
        ChessFigure c, c2;

        if (isWhite)
        {
            // Diagonal Left
            if(CurrentX != 0 && CurrentY != 7)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX -1, CurrentY +1];
                if(c != null && !c.isWhite) r[CurrentX - 1, CurrentY + 1] = true;
            }

            // Diagonal Right
            if (CurrentX != 7 && CurrentY != 7)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX + 1, CurrentY + 1];
                if (c != null && !c.isWhite) r[CurrentX + 1, CurrentY + 1] = true;
            }

            // Forward
            if(CurrentY != 7)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX, CurrentY + 1];
                if(c == null) r[CurrentX, CurrentY + 1] = true;
            }
            // Two Steps Forward
            if(CurrentY == 1)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX, CurrentY + 1];
                c2 = BoardManager.Instance.ChessFigurePositions[CurrentX, CurrentY + 2];
                if(c == null && c2 == null) r[CurrentX, CurrentY + 2] = true;
            }
        }
        else
        {
            // Diagonal Left
            if (CurrentX != 0 && CurrentY != 0)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX - 1, CurrentY - 1];
                if (c != null && c.isWhite) r[CurrentX - 1, CurrentY - 1] = true;
            }

            // Diagonal Right
            if (CurrentX != 7 && CurrentY != 0)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX + 1, CurrentY - 1];
                if (c != null && c.isWhite) r[CurrentX + 1, CurrentY - 1] = true;
            }

            // Forward
            if (CurrentY != 0)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX, CurrentY - 1];
                if (c == null) r[CurrentX, CurrentY - 1] = true;
            }
            // Two Steps Forward
            if (CurrentY == 6)
            {
                c = BoardManager.Instance.ChessFigurePositions[CurrentX, CurrentY - 1];
                c2 = BoardManager.Instance.ChessFigurePositions[CurrentX, CurrentY - 2];
                if (c == null && c2 == null) r[CurrentX, CurrentY - 2] = true;
            }
        }

        return r;
    }
}</pre>

The next character I&#8217;m going to implement will be the bishop. The bishop can run any number of fields diagonally, as long as no other figures stand in the way.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class Bishop : ChessFigure
{
    public override bool[,] PossibleMove()
    {
        bool[,] r = new bool[8, 8];

        ChessFigure c;
        int i, j;

        // Top Left
        i = CurrentX;
        j = CurrentY;
        while(true)
        {
            i--;
            j++;
            if (i &lt; 0 || j &gt;= 8) break;
            c = BoardManager.Instance.ChessFigurePositions[i, j];
            if (c == null) r[i, j] = true;
            else
            {
                if (c.isWhite != isWhite) r[i, j] = true;
                break;
            }
        }

        // Top Right
        i = CurrentX;
        j = CurrentY;
        while (true)
        {
            i++;
            j++;
            if (i &gt;= 8 || j &gt;= 8) break;
            c = BoardManager.Instance.ChessFigurePositions[i, j];
            if (c == null) r[i, j] = true;
            else
            {
                if (c.isWhite != isWhite) r[i, j] = true;
                break;
            }
        }

        // Bottom Left
        i = CurrentX;
        j = CurrentY;
        while (true)
        {
            i--;
            j--;
            if (i &lt; 0 || j &lt; 0) break;
            c = BoardManager.Instance.ChessFigurePositions[i, j];
            if (c == null) r[i, j] = true;
            else
            {
                if (c.isWhite != isWhite) r[i, j] = true;
                break;
            }
        }

        // Bottom Right
        i = CurrentX;
        j = CurrentY;
        while (true)
        {
            i++;
            j--;
            if (i &gt;= 8 || j &lt; 0) break;
            c = BoardManager.Instance.ChessFigurePositions[i, j];
            if (c == null) r[i, j] = true;
            else
            {
                if (c.isWhite != isWhite) r[i, j] = true;
                break;
            }
        }

        return r;
    }
}</pre>

The rook is implemented in a similar way to the bishop, but the movement here is straight (vertical and horizontal) instead of diagonal.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class Rook : ChessFigure
{
    public override bool[,] PossibleMove()
    {
        bool[,] r = new bool[8, 8];
        ChessFigure c;
        int i;

        // Left
        i = CurrentX;
        while(true)
        {
            i--;
            if (i &lt; 0) break;
            c = BoardManager.Instance.ChessFigurePositions[i, CurrentY];
            if (c == null) r[i, CurrentY] = true;
            else
            {
                if(c.isWhite != isWhite) r[i, CurrentY] = true;
                break;
            }
        }

        // Right
        i = CurrentX;
        while (true)
        {
            i++;
            if (i &gt;= 8) break;
            c = BoardManager.Instance.ChessFigurePositions[i, CurrentY];
            if (c == null) r[i, CurrentY] = true;
            else
            {
                if (c.isWhite != isWhite) r[i, CurrentY] = true;
                break;
            }
        }

        // Forward
        i = CurrentY;
        while (true)
        {
            i++;
            if (i &gt;= 8) break;
            c = BoardManager.Instance.ChessFigurePositions[CurrentX, i];
            if(c == null) r[CurrentX, i] = true;
            else
            {
                if(c.isWhite != isWhite) r[CurrentX, i] = true;
                break;
            }
        }

        // Back
        i = CurrentY;
        while (true)
        {
            i--;
            if (i &lt; 0) break;
            c = BoardManager.Instance.ChessFigurePositions[CurrentX, i];
            if (c == null) r[CurrentX, i] = true;
            else
            {
                if (c.isWhite != isWhite) r[CurrentX, i] = true;
                break;
            }
        }

        return r;
    }
}</pre>

Another complicated looking figure is the knight. The knight moves two fields straight ahead in one direction, followed by a 90 degree turn and a further movement by one field in this direction. Alternatively, the movement of a single field can be done first, followed by the two-field-movement. Figures that are on the fields within the movement sequence are irrelevant. However, the knight has a positive side: the movements do not have to be implemented separately for both colors.

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class Knight : ChessFigure
{
    public override bool[,] PossibleMove()
    {
        bool[,] r = new bool[8, 8];

        // Up / Left
        KnightMove(CurrentX - 1, CurrentY + 2, ref r);
        KnightMove(CurrentX - 2, CurrentY + 1, ref r);

        // Up / Right
        KnightMove(CurrentX + 1, CurrentY + 2, ref r);
        KnightMove(CurrentX + 2, CurrentY + 1, ref r);

        // Down / Left
        KnightMove(CurrentX - 1, CurrentY - 2, ref r);
        KnightMove(CurrentX - 2, CurrentY - 1, ref r);

        // Down / Right
        KnightMove(CurrentX + 1, CurrentY - 2, ref r);
        KnightMove(CurrentX + 2, CurrentY - 1, ref r);

        return r;
    }

    public void KnightMove(int x, int y, ref bool[,] r)
    {
        ChessFigure c;
        if(x &gt;= 0 && x &lt; 8 && y &gt;= 0 && y &lt; 8)
        {
            c = BoardManager.Instance.ChessFigurePositions[x, y];
            if (c == null) r[x, y] = true;
            else if (c.isWhite != isWhite) r[x, y] = true;
        }
    }
}</pre>

The possible movements of the king correspond to two fields in any direction, which I have implemented as follows:

<pre class="EnlighterJSRAW" data-enlighter-language="csharp">public class King : ChessFigure
{
    public override bool[,] PossibleMove()
    {
        bool[,] r = new bool[8, 8];

        ChessFigure c;
        int i, j;

        // Top
        i = CurrentX - 1;
        j = CurrentY + 1;
        if(CurrentY &lt; 7)
        {
            for(int k = 0; k &lt; 3; k++)
            {
                if(i &gt;= 0 && i &lt; 8)
                {
                    c = BoardManager.Instance.ChessFigurePositions[i, j];
                    if (c == null) r[i, j] = true;
                    else if (c.isWhite != isWhite) r[i, j] = true;
                }
                i++;
            } 
        }

        // Bottom
        i = CurrentX - 1;
        j = CurrentY - 1;
        if (CurrentY &gt; 0)
        {
            for (int k = 0; k &lt; 3; k++)
            {
                if (i &gt;= 0 && i &lt; 8)
                {
                    c = BoardManager.Instance.ChessFigurePositions[i, j];
                    if (c == null) r[i, j] = true;
                    else if (c.isWhite != isWhite) r[i, j] = true;
                }
                i++;
            }
        }

        // Left
        if(CurrentX &gt; 0)
        {
            c = BoardManager.Instance.ChessFigurePositions[CurrentX - 1, CurrentY];
            if (c == null) r[CurrentX - 1, CurrentY] = true;
            else if (c.isWhite != isWhite) r[CurrentX - 1, CurrentY] = true;
        }

        // Right
        if (CurrentX &lt; 7)
        {
            c = BoardManager.Instance.ChessFigurePositions[CurrentX + 1, CurrentY];
            if (c == null) r[CurrentX + 1, CurrentY] = true;
            else if (c.isWhite != isWhite) r[CurrentX + 1, CurrentY] = true;
        }

        return r;
    }
}</pre>

And last but not least, the queen&#8217;s movements are still missing. These seem to be quite complex again, but here you can simply combine the already created movements of rook and bishop, which is why I don&#8217;t separately add a snippet of the source code for the queens implementation.

Now that&#8217;s it! The functionality of the MVP defined at the beginning is fully implemented. I will now try to implement a simple AI, which will help you to play the game without disturbing social interaction. Stay tuned!