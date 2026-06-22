---
name: github-skills-access
description: Gebruik deze skill alleen wanneer Jelle expliciet vraagt om iets met GitHub of de skills-repo te doen, bijvoorbeeld "zet dit op GitHub", "voeg deze skill toe aan de repo", of "push dit naar Yelbow/claude-skills". Trigger niet bij normaal skills lezen, gebruiken, of aanpassen binnen de chat. Alleen actief bij een expliciete GitHub-actie.
---

# GitHub Skills Access

## Wanneer te gebruiken
Alleen bij een expliciete vraag om iets met GitHub te doen. Skills lezen, gebruiken of bespreken in de chat heeft dit niet nodig.

## GitHub config
Token en repo-info (Yelbow/claude-skills) staan in Google Drive, bestand "claude-config", file ID 1lYU--uCLynzvru52H-7gWB70dGRDZX01.

Fetch dit bestand alleen op het moment dat een GitHub-actie wordt gevraagd. Niet automatisch bij sessiestart.

## Externe/community skills
Skills van externe bronnen (bijvoorbeeld Matt Pocock's repo) worden verbatim toegevoegd aan `skills/[naam]/SKILL.md`. Niet vertalen of aanpassen zonder het eerst te vragen.
