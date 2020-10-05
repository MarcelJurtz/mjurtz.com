---
title: 'Fluentvalidation in ASP.NET Core'
date: 2019-10-05T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2020/10/fluentvalidation-in-asp-net-core
categories:
  - csharp
---

Hast du die Validation deiner ViewModels in ASP.NET bisher mithilfe von DataAttributes umgesetzt? Warst du dabei auch immer unzufrieden, da deine Klassen unübersichtlich und voll von unnötig repliziertem Code geworden sind? Dann ist der heutige Post für dich.

## FluentValidation

FluentValidation ist eine Möglichkeit, Validation für Klassen in Form einer Fluent-API zu definieren. Fluent bezieht sich dabei auf die Eigenschaft, dass Funktionen jeweils das geänderte Objekt zurückgeben. Das ermöglicht es dir, die Methodenaufrufe zu verketten und sorgt (oft) für sehr gute Lesbarkeit, vor allem im Bereich von Konfigurationen, wo es eben häufig verwendet wird. Du kennst die Struktur auch möglicherweise aus Bereichen wie dem ASP.NET Setup oder aus LINQ.

FluentValidation ist eine beliebte Library zur Definition von Validationsregeln in .NET und ist Open Source auf [GitHub](https://github.com/FluentValidation) verfügbar. Vorab: Die [Dokumentation](https://docs.fluentvalidation.net/en/latest/index.html) ist sehr gut und sollte für die meisten Fragen mehr als ausreichen.

## Installation & Einrichtung

Um FluentValidation verwenden zu können, muss zuerst das entsprechende NuGet-Paket *FluentValidation.AspNetCore* installiert werden. Danach fehlt nur noch die Konfiguration im Startup-Bereich der Anwendung. Füge hierzu einfach in der Methode *ConfigureServices* einen Aufruf von *AddFluentValidation* ein. Um nun noch korrekt mit der DI-Infrastruktur zu funktionieren, müssen die Validatoren hier anschließend noch definiert werden:

```csharp
public void ConfigureServices(IServiceCollection services) {
    services.AddMvc()
        // ...
        .AddFluentValidation();
    
    services.AddTransient<IValidator<ClubMember>, ClubMemberValidator>();
    // ...
}
```

Das wird natürlich sehr unübersichtlich und unnötig kompliziert, wenn viele Validatoren verwendet werden. Zum Glück gibt es aber auch eine alternative Möglichkeit: Die automatische Registrierung aller Validatoren innerhalb einer definierten Assembly, die sowohl als öffentlich markiert sind, als auch von AbstractValidator erben.

Anschließend kann für jede Entität, die zu validieren ist, einfach eine Validator-Klasse definiert werden, welche die Prüfungslogik enthält. Diese könnte beispielsweise so aussehen:

```csharp
public class ClubMember {
	public int Id { get; set; }
    public int MemberId { get; set; }
    public int ClubId { get; set; }
    public string Role { get; set; }
    public int Number { get; set; }
}

public class ClubMemberValidator : AbstractValidator<ClubMember> {
	public ClubMemberValidator() {
		RuleFor(x => x.Role).Length(3, 25);
		RuleFor(x => x.Number).InclusiveBetween(1, 100);
        // ...
	}
}
```

Es stehen eine Reihe von vordefinierten Regeln zur Verfügung, eigene können aber jederzeit definiert werden. Die Validation selbst ist nun eingerichtet und greif, bzw. kann im Controller über *ModelState.IsValid* abgerufen werden. Durch die generische Definition wird der Validator automatisch mit der Entitätsklasse verknüpft.

## Fallstricke

Es gibt ein paar Punkte, die bei der Verwendung zu beachten sind. So kann beispielsweise die reguläre Validation mittels DataAttributes weiterhin verwendet werden, da die reguläre Validation nach der Validation über FluentValidation zusätzlich greift. Es hilft hier also, alten Code aufzuräumen um unerwartete Nebeneffekte zu vermeiden.

Ein weiterer Punkt ist, dass komplexe Objekte nicht direkt in deren hierarchischen Struktur nach unten hin weiter geprüft werden. Wenn du möchtest, dass Child-Objekte direkt validiert werden, musst du das entweder auf dem Objekt definieren, oder in der Konfiguration grundsätzlich hinterlegen. Dazu gibt es dort das Property *ImplicitlyValidateChildProperties*.

FluentValidation selbst dient der serverseitigen Validation. Es liefert jedoch auch Metadaten zur Validation auf Client-Seite. Genauere Informationen zur Nutzung der Validatoren für clientseitige Validationsbibliotheken findest du in der Projektdokumentation.

## Fazit

Das soll es erstmal zur Einrichtung gewesen sein. FluentValidation bietet meiner Meinung nach eine sehr schöne Möglichkeit zur Definition von Validationsregeln in ASP.NET (Core). Es gibt aber noch viele weitere Details, auf die ich noch nicht eingegangen bin. Wie bereits erwähnt ist die Dokumentation des Projekts eine sehr gute Anlaufstelle für Antworten auf potentielle Fragen und allgemeine Information, möglicherweise folgt hier aber auch ein weiterer Post mit Punkten zu verschiedenen Details.

