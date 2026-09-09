---
title: Warum KI-Agenten trotz Preisverfall teurer werden
date: 2026-09-09 07:47:00 +/-0000
categories: [Strategie, Compliance, KI]
tags: [ki,open-weights,gpu-hosting,souveränität] # TAG names should always be lowercase
image:
  path: https://picsur.kngstn.eu/i/1e99f86e-eb59-471f-bde2-67ccfb0ee0ad.png
---

Die Preise für die großen KI-Modelle fallen seit zwei Jahren praktisch im Halbjahrestakt. Trotzdem prognostiziert Gartner, dass sich die laufenden Kosten pro agentenbasiertem Workflow bis 2028 etwa verfünffachen. Der Grund für dieses Schein-Paradoxon liegt in der Architektur agentischer Systeme.

## Warum günstigere Modelle die Rechnung nicht senken

Ein einfacher Chatbot beantwortet eine Frage und ist fertig. Ein KI-Agent dagegen zieht laufend Schlussfolgerungen, verhandelt zwischen einzelnen Aufgaben und hinterfragt eigene Ergebnisse, bevor er weitermacht, so beschreibt es Gartner-Analyst Will Sommer den Unterschied. Jeder dieser Zwischenschritte kostet Tokens, also das, womit wir den Agenten bezahlen. Allerdings macht ein Agent auch deutlich mehr als ein reiner Chatbot mit seiner einzelnen Antwort.

Genau hier liegt der Widerspruch. Weil Tokenpakete jetzt billiger werden, wird es wirtschaftlich interessanter, komplexere Workflows mit mehr Zwischenschritten zu bauen. Unternehmen, die diesen Spielraum nutzen, stellen schnell fest, dass der Tokenverbrauch pro Anwendungsfall schneller steigt, als der Preis pro Token fällt. Am Ende fällt die Rechnung höher aus, als es der niedrige Preis pro Token vermuten lässt. Gartner geht davon aus, dass deshalb bis 2027 über 40 Prozent aller geplanten oder bereits eingesetzten KI-Agenten wieder eingestellt werden, nicht wegen schlechter Technik, sondern wegen steigender Kosten und ausbleibender Rentabilität.

![Token-Preis vs Token-Verbrauch Agenten-Workflow](https://picsur.kngstn.eu/i/ad2cdf28-fffe-4fde-9fb9-ea249c21ddb3.png)

Die Empfehlung von Gartner ist entsprechend nüchtern, ohne eine abgestufte Modellnutzung, Routing zwischen den verschieden teuren Modellen und ein bewusst orchestriertes Ökosystem aus mehreren Modellen drohen unbegrenzte Kosten.

## Ein Baustein, den Gartner nicht adressiert

Parallel zur Modell-Optimierung existiert eine zweite Stellschraube, der Wechsel von variablen Token-APIs zu dedizierter Infrastruktur. Viele Unternehmen zahlen für jede einzelne Anfrage an einen fremden API-Anbieter, obwohl es für einen großen Teil ihrer Anwendungsfälle inzwischen leistungsfähige Modelle gibt, die man selbst betreiben kann, mit festen Infrastrukturkosten statt linear wachsender Abrechnung von Tokens.

Besonders bemerkenswert dabei ist, ein Teil der stärksten frei verfügbaren, selbst hostbaren Modelle kommt inzwischen aus China. DeepSeek, Qwen und Kimi liegen qualitativ nur noch etwa zwei bis drei Monate hinter der US-Spitze, DeepSeek hat mit einem Bruchteil der Kosten der großen US-Anbieter, ein konkurrenzfähiges Modell gebaut. Für viele Standardanwendungen im Unternehmen reicht dieser Abstand längst nicht mehr, um einen Unterschied zu machen.

## Chinesisches Modell, europäischer Betrieb

Der Einwand, den ich immer wieder in diesem Zusammenhang höre ist,  dass ein chinesisches Modell im eigenen Unternehmen einzusetzen, nach einem neuen Souveränitätsproblem statt einer Lösung klingt. Meist kommt dann auch der Vergleich mit dem "Hosted in Europe" Problem, das wir aus der US-Cloud-Debatte kennen. Aber dieser Vergleich hinkt hier technisch.

Bei einem Open-Weight-Modell laden wir das Modell einmal herunter und betreiben es danach vollständig in der eigenen oder einer gehosteten Infrastruktur. Es gibt keine laufende Verbindung zurück zum Modell-Entwickler, anders als bei einem API-Aufruf gegen einen US-Hyperscaler, wo der CLOUD Act unabhängig vom Serverstandort greift, wie Microsoft Frankreich kürzlich öffentlich bestätigt hat.

Es gibt bereits Anbieter in Europa, die in ihren Rechenzentren DeepSeek, Qwen oder Kimi komplett abgeschottet betreiben. Per Firewall von jeder Verbindung zu den Modell-Entwicklern getrennt, somit bleibt die gesamte Kommunikation auf den eigenen Servern. Die Trennung von Modell-Herkunft und Betriebs-Souveränität kann also auch sauber technisch umgesetzt werden.

## Wo ich das bei uns sehe

Previder betreibt seine Infrastruktur bereits vollständig in eigenen, 100 Prozent niederländischen Rechenzentren, ISO27001- und NEN7510-zertifiziert, DSGVO-konform, ohne US-CLOUD-Act-Exposition und ohne Lock-in in einen amerikanischen Hyperscaler. Der konkrete nächste Schritt für Sie ist die Frage, ob unsere Umgebung, mit dem Anspruch "Hosted in EU" nicht nur auf den Serverstandort bezogen, sondern auf den gesamten Betrieb der Modelle auch Ihren Anforderungen entspricht.

Das ist an dieser Stelle meine persönliche Einschätzung. Ich sehe darin eine der interessantesten Möglichkeiten, wie ein Managed-Hosting-Anbieter wie wir die Souveränitätsdebatte und die Kostenexplosion bei Agenten gleichzeitig adressieren kann, indem wettbewerbsfähige offene Modelle, unabhängig von ihrem Entwicklungsland, in einer vollständig kontrollierten, europäischen Umgebung angeboten werden können.

## Wofür sich das konkret lohnt

Die Datenkategorien, um die es in der Praxis geht, sind Geschäftszahlen, Konstruktionswissen, Patente oder das interne QM-Handbuch. Das sind exakt die Fälle, bei denen ein selbst betriebenes offenes Modell den größten Unterschied macht, nicht als Ersatz für jeden KI-Anwendungsfall im Unternehmen, sondern gezielt dort, wo Daten das eigene Haus grundsätzlich nicht verlassen dürfen oder sollen.

Konkret heißt das, interne Wissenssuche über Konstruktionsunterlagen und Patente, Vorprüfung von QM-Dokumenten, Auswertung von Finanzkennzahlen, Anwendungsfälle, bei denen Sie heute vermutlich zögern, sie überhaupt an eine externe API zu schicken. Ein selbst gehostetes offenes Modell verschiebt diese Abwägung, weil die Daten den kontrollierten Rahmen gar nicht erst verlassen, auch nicht zum externen Training.

Für weniger sensible, dafür aber besonders agentenlastige Workflows, also genau die Fälle, die laut Gartner die Kosten explodieren lassen, lohnt sich zusätzlich eine nüchterne Rechnung. Eine feste Infrastrukturkosten-Basis für ein selbst betriebenes Modell gegen eine linear mit jedem Agentenschritt wachsende API-Rechnung. Ab einem bestimmten Nutzungsvolumen kippt diese Rechnung, und zwar unabhängig davon, ob das eingesetzte Modell aus den USA, Europa oder China stammt.

Diese Rechnung kippt allerdings nicht von selbst. Modelle wie DeepSeek oder Qwen in ihren stärksten Varianten bringen es auf 70 Milliarden Parameter und mehr, das läuft nicht auf Standard-Servern, sondern braucht GPU-Cluster auf Basis von Karten wie NVIDIA H100, H200 oder B200. Wer aber selbst hostet, tauscht variable Tokenkosten gegen feste GPU-Kosten und übernimmt zusätzlich Verantwortung für Dinge, die bei einem API-Aufruf unsichtbar bleiben, wie zum Beispiel ausreichend VRAM für mehrere gleichzeitige Anfragen, Latenz unter Last oder Verfügbarkeit der GPU-Hardware selbst. Dieser Aufwand verschwindet nicht, er verschiebt sich nur vom eigenen Rechenzentrum zu einem Managed-Hosting-Anbieter, der genau darauf spezialisiert ist. Wirtschaftlich lohnt sich das Ganze zudem nur mit ausreichender Grundauslastung. Ein GPU-Cluster kostet unabhängig davon, ob er läuft oder "leer steht". Erst eine dauerhafte Anfragelast bringt die festen Hardwarekosten unter das, was dieselbe Last über Token-APIs kosten würde.

Um hier fair zu sein, es gibt keine wirklich belastbare Beispielrechnung. Die Zahlen, die zu finden sind, schwanken um das 10 - 100-fache je nachdem welche Annahmen man trifft. Ich möchte aber trotzdem hier meine Gedanken und meine Rechnung teilen.

![Break-Even Self-Hosting vs Token-API](https://picsur.kngstn.eu/i/8064cdd9-2454-4631-b16c-6d9617c089da.png)

Wenn ich von Hosting ausgehe, entstehen Fixkosten in Höhe von ca. 15.000 $/Monat (8×H100, Mittelwert ca. 20 $/Std.), daraus ergeben sich ca. 0,15 $/Mio. Tokens inkl.  einem Aufschlag für Redundanz. Der Break-even gegenüber einer API-Variante ist ab ca. 50 % Auslastung erreicht, ab ca. 70 % Dauerauslastung 3- bis 5-fach günstiger. Je teurer der Vergleichs-Anbieter, desto schneller kippt die Rechnung zugunsten von Self-Hosting. Das ist jetzt keine belastbare Studie, sondern lediglich zur Veranschaulichung, da ich hier weder VRAM noch die unterschiedlichen Strompreise (USA im Vergleich zu Europa) berücksichtige. In den Quellen finden Sie noch einen weiterführenden Artikel, der etwas tiefer auf diesen Vergleich eingeht.

Wenn Sie prüfen wollen, ob sich für Ihre KI-Agenten-Workflows der Wechsel auf ein selbst gehostetes, offenes Modell lohnt, oder wo Ihre sensibelsten Daten heute noch unnötig eine externe API erreichen, melden Sie sich gerne über [LinkedIn](https://www.linkedin.com/in/thomaskrampe) oder per [E-Mail](mailto:t.krampe@previder.de). Ich würde mich gern völlig unverbindlich mit Ihnen austauschen.

## Quellen

- [heise online: Prognose: Inferenzkosten für KI-Agenten verfünffachen sich bis 2028 (Gartner)](https://www.heise.de/news/Prognose-Inferenzkosten-fuer-KI-Agenten-verfuenffachen-sich-bis-2028-11417470.html)
- [KI-Allianz Baden-Württemberg: KI-Hosting und digitale Souveränität](https://ki-allianz.de/ki-hosting-deutschland-souveraenitaet/)
- [Retarus Blog: Schwarz auf weiß — Microsoft kann nicht garantieren, dass europäische Daten in Europa bleiben](https://www.retarus.com/blog/de/schwarz-auf-weiss-microsoft-kann-nicht-garantieren-dass-europaeische-daten-in-europa-bleiben/)
- [Self-Hosting an LLM vs. API: Real Cost Math (2026) — Cloudzy](https://cloudzy.com/blog/self-hosting-open-weight-llm-gpu-vps-cost/)
