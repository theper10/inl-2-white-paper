[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/wfopJe1F)

# Inlämning 2 - White Paper

## Sammanfattning

Det här white papret handlar om hur ett företag kan få bättre kontroll på sina releaser och minska risken att fel hamnar i produktion. I mitt exempel utgår jag från ett påhittat företag som heter CareFlow. De jobbar med digital vårdbokning och videosamtal med vårdpersonal. Företaget har haft problem med att nya versioner ibland släpps med buggar, vilket skapar stress internt och problem för användarna.

Min lösning är att företaget behöver en tydligare release-process där viktiga kontroller sker automatiskt innan något släpps vidare. Tanken är att koden inte ska kunna gå hela vägen till produktion om tester misslyckas eller om bygget inte fungerar. På så sätt blir pipelinen en säkerhetskontroll mellan utveckling och produktion.

Jag föreslår att företaget jobbar med automatiserade enhetstester, integrationstester och ett mindre antal E2E-tester. Jag föreslår också att de använder Continuous Delivery i stället för Continuous Deployment. Det passar bättre här eftersom verksamheten är känslig och det är klokt att ha ett sista godkännande innan något går ut till användarna.

Utöver detta behöver företaget ha en plan för rollback, tydlig ansvarsfördelning och säker hantering av känslig information. Målet är en lugnare och mer stabil releaseprocess där färre fel når slutanvändarna.

## Företagsbeskrivning

CareFlow är ett fiktivt företag som erbjuder digital vårdbokning. Användare kan boka tider, få påminnelser och genomföra videosamtal med vårdpersonal via tjänsten. Företaget har både en webbapplikation och ett backend-system som hanterar bokningar, användaruppgifter och meddelanden.

Eftersom tjänsten används i vårdsammanhang är det viktigt att den fungerar stabilt. Om systemet ligger nere eller om något viktigt slutar fungera påverkas användarna direkt. Därför behöver företaget ha en releaseprocess som är trygg och genomtänkt.

## Bakgrund och problem

CareFlow har hamnat i en situation där buggar ibland släpps till produktion. En viktig orsak är att teamet ofta jobbar under tidspress. Det finns ett starkt fokus på att få ut nya funktioner snabbt, men inte lika tydliga krav på kvalitet innan release.

Testningen är också ojämn. Vissa delar av systemet testas ordentligt, medan andra mest kontrolleras manuellt. Det gör att fel lätt kan missas. Företaget saknar dessutom tydliga regler för när en release ska stoppas. Om ansvarsfördelningen är otydlig blir det lätt att alla tror att någon annan redan har kollat.

Det här leder till att företaget ofta får jobba reaktivt. I stället för att kunna planera framåt måste teamet lägga tid på att rätta akuta fel efter att något redan har släppts.

## Lösningsförslag

Jag föreslår att företaget inför en tydlig release-pipeline med automatiska kontroller. Målet är att varje ändring ska passera samma kedja av tester och kontroller innan den får gå vidare.

Företaget skulle till exempel kunna ha en teknisk miljö med:
- frontend i JavaScript eller TypeScript
- backend i C# eller Node.js
- en databas, till exempel PostgreSQL
- GitHub för versionshantering
- GitHub Actions för pipeline

Det viktigaste är dock inte exakt vilka verktyg som används, utan att processen är tydlig och konsekvent. Företaget behöver få på plats:
- automatiserade tester
- kontroll av att builden fungerar
- en staging-miljö för sista kontroll
- tydliga quality gates
- möjlighet att stoppa eller backa en release

Det behövs också tydligare ansvar i teamet. Utvecklarna behöver ta ansvar för testerna kring sin kod. Teamet behöver ha kodgranskning som en fast del av arbetet. Den som ansvarar för drift eller DevOps behöver se till att pipelinen och deploymenten fungerar på ett säkert sätt.

## Release-pipeline steg för steg

En möjlig releasekedja för CareFlow kan se ut så här:

1. En utvecklare gör en ändring och pushar koden till repot.
2. Pipelinen startar automatiskt.
3. Enhetstester körs först eftersom de går snabbt och kan fånga enkla fel tidigt.
4. Om testerna går igenom byggs applikationen.
5. Därefter körs integrationstester för att kontrollera att olika delar fungerar tillsammans.
6. Om det behövs körs också några viktiga E2E-tester.
7. Om allt ser bra ut deployas versionen till en staging-miljö.
8. I staging görs en sista kontroll i en miljö som liknar produktion.
9. Först därefter godkänns release till produktion.

Det här är en enkel men tydlig kedja som minskar risken att felaktig kod går hela vägen ut till användarna.

## Continuous Delivery vs Continuous Deployment

Continuous Delivery betyder att koden automatiskt byggs, testas och görs redo för release, men att själva produktionssättningen fortfarande kräver ett aktivt beslut. Det finns alltså ett sista steg där någon godkänner att releasen ska gå ut.

Continuous Deployment betyder att allt som klarar pipelinen automatiskt går hela vägen till produktion utan manuellt godkännande.

För CareFlow tycker jag att Continuous Delivery passar bäst. Eftersom företaget arbetar med en känslig tjänst känns det rimligt att ha kvar ett sista kontrollsteg innan något når användarna. Det ger fortfarande en hög grad av automatisering, men med lite mer trygghet.

## Exempel på trigger i YAML

Nedan är ett enkelt exempel på hur en pipeline kan starta automatiskt när kod pushas till main:

```yaml
name: Release Pipeline

on:
  push:
    branches:
      - main

jobs:
  test-and-build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run tests
        run: npm test

      - name: Build app
        run: npm run build
```

Poängen med det här exemplet är att visa att pipelinen kan starta automatiskt när ny kod skickas upp till repot.

## Teststrategi

Jag tycker att företaget ska lägga mest fokus på enhetstester. De är snabba att köra och bra på att hitta fel tidigt. De passar bra för att testa mindre delar av koden och viktig logik.

Integrationstester behövs också eftersom de visar om olika delar av systemet fungerar tillsammans. Det kan till exempel handla om att kontrollera att backend och databas fungerar ihop som tänkt.

E2E-tester är också användbara, men jag tycker inte att företaget ska ha för många sådana. De är ofta långsammare och känsligare än andra tester. Därför är det smartare att använda dem för några få viktiga flöden, till exempel inloggning eller bokning av ett vårdbesök.

Min prioritering hade därför varit:
1. många enhetstester
2. ett rimligt antal integrationstester
3. några få viktiga E2E-tester

## Quality Gates

Quality gates är regler som avgör om en release får gå vidare eller inte. De är viktiga för att pipelinen inte bara ska vara automatisk, utan också pålitlig.

Exempel på quality gates i CareFlow skulle kunna vara:
- att enhetstesterna måste gå igenom
- att integrationstesterna inte får fallera
- att builden måste lyckas
- att kodgranskning måste vara godkänd
- att deployment till staging måste fungera innan produktion

Om något av detta inte fungerar ska releasen stoppas automatiskt.

Det är också viktigt att förstå att tester inte alltid har rätt. Ibland kan ett test ge grönt fast det egentligen finns ett problem. Därför räcker det inte att bara ha tester. Företaget måste också se till att testerna hålls uppdaterade och faktiskt testar rätt saker.

## Riskminimering och rollback

Det finns olika sätt att minska risken när man släpper en ny version. Två vanliga strategier är Blue/Green Deployment och Canary Release.

Blue/Green Deployment betyder att man har två miljöer. En kör den nuvarande versionen och den andra den nya. När den nya versionen har kontrollerats kan trafiken flyttas över. Om något går fel går det snabbt att byta tillbaka.

Canary Release betyder att den nya versionen först släpps till en mindre grupp användare. Om allt fungerar bra kan man stegvis släppa den till fler.

För CareFlow tycker jag att Blue/Green känns mest passande. Den är ganska enkel att förstå och gör det lätt att backa snabbt om något blir fel.

Rollback betyder att man går tillbaka till en tidigare stabil version. Företaget bör därför ha en tydlig plan för hur det ska ske. Det är särskilt viktigt i de fall där deploymenten tekniskt sett lyckas, men där användarna ändå märker att något inte fungerar som det ska.

## Säkerhet i pipelinen

Säkerhet är en viktig del av hela releaseprocessen. Företaget ska inte lägga lösenord, API-nycklar eller annan känslig information direkt i koden. Sådana uppgifter bör i stället sparas som secrets i det system som används för pipeline och deployment.

Det är också viktigt att inte ge för breda behörigheter. En pipeline eller tjänst ska bara ha den åtkomst som faktiskt behövs. På så sätt minskar risken för misstag och onödiga säkerhetsproblem.

## Egen kompetensanalys

Genom den här uppgiften har jag fått en bättre förståelse för att DevOps inte bara handlar om att automatisera saker, utan också om att skapa ordning, tydlighet och trygghet i hur kod släpps.

Jag har också lärt mig att en bra releaseprocess handlar mycket om att förebygga problem, inte bara lösa dem när de redan har hänt. Det är något jag tror är viktigt i arbetslivet, särskilt när flera personer jobbar i samma system.

Det här är kunskap som jag tror kan vara användbar både på praktik och i framtida jobb, eftersom det visar att jag inte bara tänker på att få kod att fungera, utan också på hur den ska testas, släppas och fungera stabilt över tid.
