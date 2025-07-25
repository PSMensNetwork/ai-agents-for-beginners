<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "8164484c16b1ed3287ef9dae9fc437c1",
  "translation_date": "2025-07-23T09:11:59+00:00",
  "source_file": "10-ai-agents-production/README.md",
  "language_code": "ro"
}
-->
# Agenți AI în Producție: Observabilitate și Evaluare

Pe măsură ce agenții AI trec de la prototipuri experimentale la aplicații reale, abilitatea de a înțelege comportamentul lor, de a monitoriza performanța și de a evalua sistematic rezultatele devine esențială.

## Obiective de Învățare

După finalizarea acestei lecții, vei ști cum să/vei înțelege:
- Conceptele de bază ale observabilității și evaluării agenților
- Tehnici pentru îmbunătățirea performanței, costurilor și eficienței agenților
- Ce și cum să evaluezi sistematic agenții AI
- Cum să controlezi costurile atunci când implementezi agenți AI în producție
- Cum să instrumentezi agenții construiți cu AutoGen

Scopul este să te echipezi cu cunoștințele necesare pentru a transforma agenții "cutie neagră" în sisteme transparente, gestionabile și de încredere.

_**Notă:** Este important să implementezi agenți AI care sunt siguri și de încredere. Consultă lecția [Construirea Agenților AI de Încredere](./06-building-trustworthy-agents/README.md) pentru mai multe detalii._

## Trasee și Etape

Instrumentele de observabilitate precum [Langfuse](https://langfuse.com/) sau [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry) reprezintă de obicei rulările agenților ca trasee și etape.

- **Traseu** reprezintă o sarcină completă a agentului de la început până la sfârșit (cum ar fi gestionarea unei interogări a utilizatorului).
- **Etape** sunt pașii individuali din cadrul traseului (cum ar fi apelarea unui model lingvistic sau recuperarea datelor).

![Arbore de trasee în Langfuse](https://langfuse.com/images/cookbook/example-autogen-evaluation/trace-tree.png)

Fără observabilitate, un agent AI poate părea o "cutie neagră" - starea sa internă și raționamentul sunt opace, ceea ce face dificilă diagnosticarea problemelor sau optimizarea performanței. Cu observabilitate, agenții devin "cutii de sticlă", oferind transparență vitală pentru construirea încrederii și asigurarea funcționării lor conform intenției.

## De ce Observabilitatea Contează în Mediile de Producție

Trecerea agenților AI în medii de producție introduce un set nou de provocări și cerințe. Observabilitatea nu mai este un "lux", ci o capacitate critică:

*   **Depanare și Analiza Cauzei Primare**: Când un agent eșuează sau produce un rezultat neașteptat, instrumentele de observabilitate oferă traseele necesare pentru identificarea sursei erorii. Acest lucru este deosebit de important în cazul agenților complexi care pot implica multiple apeluri LLM, interacțiuni cu instrumente și logică condițională.
*   **Gestionarea Latentei și Costurilor**: Agenții AI se bazează adesea pe LLM-uri și alte API-uri externe care sunt facturate pe token sau pe apel. Observabilitatea permite urmărirea precisă a acestor apeluri, ajutând la identificarea operațiunilor excesiv de lente sau costisitoare. Acest lucru permite echipelor să optimizeze prompturile, să selecteze modele mai eficiente sau să redeseneze fluxurile de lucru pentru a gestiona costurile operaționale și a asigura o experiență bună pentru utilizator.
*   **Încredere, Siguranță și Conformitate**: În multe aplicații, este important să se asigure că agenții se comportă în mod sigur și etic. Observabilitatea oferă o pistă de audit a acțiunilor și deciziilor agentului. Aceasta poate fi utilizată pentru a detecta și atenua probleme precum injectarea de prompturi, generarea de conținut dăunător sau gestionarea necorespunzătoare a informațiilor personale identificabile (PII). De exemplu, poți revizui traseele pentru a înțelege de ce un agent a oferit un anumit răspuns sau a utilizat un instrument specific.
*   **Buclă de Îmbunătățire Continuă**: Datele de observabilitate sunt fundamentul unui proces de dezvoltare iterativ. Prin monitorizarea modului în care agenții performează în lumea reală, echipele pot identifica zonele de îmbunătățire, pot colecta date pentru ajustarea modelelor și pot valida impactul modificărilor. Acest lucru creează o buclă de feedback în care informațiile din evaluarea online informează experimentarea offline și rafinarea, conducând la o performanță progresiv mai bună a agentului.

## Metrice Cheie de Urmărit

Pentru a monitoriza și înțelege comportamentul agentului, trebuie urmărită o gamă de metrici și semnale. Deși metricele specifice pot varia în funcție de scopul agentului, unele sunt universal importante.

Iată câteva dintre cele mai comune metrici pe care instrumentele de observabilitate le monitorizează:

**Latenta:** Cât de rapid răspunde agentul? Timpurile lungi de așteptare afectează negativ experiența utilizatorului. Ar trebui să măsori latenta pentru sarcini și pași individuali prin trasarea rulărilor agentului. De exemplu, un agent care durează 20 de secunde pentru toate apelurile modelului ar putea fi accelerat prin utilizarea unui model mai rapid sau prin rularea apelurilor modelului în paralel.

**Costuri:** Care este costul per rulare a agentului? Agenții AI se bazează pe apeluri LLM facturate pe token sau API-uri externe. Utilizarea frecventă a instrumentelor sau multiplele prompturi pot crește rapid costurile. De exemplu, dacă un agent apelează un LLM de cinci ori pentru o îmbunătățire marginală a calității, trebuie să evaluezi dacă costul este justificat sau dacă ai putea reduce numărul de apeluri sau utiliza un model mai ieftin. Monitorizarea în timp real poate ajuta, de asemenea, la identificarea creșterilor neașteptate (de exemplu, erori care cauzează bucle excesive de API).

**Erori de Cerere:** Câte cereri a eșuat agentul? Acestea pot include erori API sau apeluri de instrumente eșuate. Pentru a face agentul mai robust în producție, poți configura soluții de rezervă sau reîncercări. De exemplu, dacă furnizorul LLM A este indisponibil, treci la furnizorul LLM B ca backup.

**Feedback-ul Utilizatorului:** Implementarea evaluărilor directe ale utilizatorilor oferă informații valoroase. Acestea pot include evaluări explicite (👍thumbs-up/👎down, ⭐1-5 stele) sau comentarii textuale. Feedback-ul negativ constant ar trebui să te alerteze, deoarece acesta este un semn că agentul nu funcționează conform așteptărilor.

**Feedback-ul Implicit al Utilizatorului:** Comportamentele utilizatorilor oferă feedback indirect chiar și fără evaluări explicite. Acestea pot include reformularea imediată a întrebărilor, interogări repetate sau apăsarea unui buton de reîncercare. De exemplu, dacă observi că utilizatorii întreabă repetat aceeași întrebare, acesta este un semn că agentul nu funcționează conform așteptărilor.

**Acuratețe:** Cât de frecvent produce agentul rezultate corecte sau dorite? Definițiile acurateței variază (de exemplu, corectitudinea rezolvării problemelor, acuratețea recuperării informațiilor, satisfacția utilizatorului). Primul pas este să definești ce înseamnă succesul pentru agentul tău. Poți urmări acuratețea prin verificări automate, scoruri de evaluare sau etichete de finalizare a sarcinilor. De exemplu, marcarea traseelor ca "reușite" sau "eșuate".

**Metrici de Evaluare Automată:** Poți configura, de asemenea, evaluări automate. De exemplu, poți utiliza un LLM pentru a evalua rezultatul agentului, de exemplu, dacă este util, precis sau nu. Există, de asemenea, mai multe biblioteci open source care te ajută să evaluezi diferite aspecte ale agentului. De exemplu, [RAGAS](https://docs.ragas.io/) pentru agenți RAG sau [LLM Guard](https://llm-guard.com/) pentru a detecta limbajul dăunător sau injectarea de prompturi.

În practică, o combinație a acestor metrici oferă cea mai bună acoperire a sănătății unui agent AI. În [notebook-ul exemplu](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) din acest capitol, vom arăta cum arată aceste metrici în exemple reale, dar mai întâi vom învăța cum arată un flux de lucru tipic de evaluare.

## Instrumentarea Agentului

Pentru a colecta date de trasare, va trebui să instrumentezi codul. Scopul este să instrumentezi codul agentului pentru a emite trasee și metrici care pot fi capturate, procesate și vizualizate de o platformă de observabilitate.

**OpenTelemetry (OTel):** [OpenTelemetry](https://opentelemetry.io/) a devenit un standard industrial pentru observabilitatea LLM. Oferă un set de API-uri, SDK-uri și instrumente pentru generarea, colectarea și exportarea datelor de telemetrie.

Există multe biblioteci de instrumentare care învelesc cadrele existente ale agenților și facilitează exportul etapelor OpenTelemetry către un instrument de observabilitate. Mai jos este un exemplu de instrumentare a unui agent AutoGen cu biblioteca de instrumentare [OpenLit](https://github.com/openlit/openlit):

```python
import openlit

openlit.init(tracer = langfuse._otel_tracer, disable_batch = True)
```

[Notebook-ul exemplu](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb) din acest capitol va demonstra cum să instrumentezi un agent AutoGen.

**Crearea Manuală a Etapelor:** Deși bibliotecile de instrumentare oferă o bază bună, există adesea cazuri în care sunt necesare informații mai detaliate sau personalizate. Poți crea manual etape pentru a adăuga logică personalizată a aplicației. Mai important, acestea pot îmbogăți etapele create automat sau manual cu atribute personalizate (cunoscute și sub numele de etichete sau metadate). Aceste atribute pot include date specifice afacerii, calcule intermediare sau orice context care ar putea fi util pentru depanare sau analiză, cum ar fi `user_id`, `session_id` sau `model_version`.

Exemplu de creare manuală a traseelor și etapelor cu [Langfuse Python SDK](https://langfuse.com/docs/sdk/python/sdk-v3):

```python
from langfuse import get_client
 
langfuse = get_client()
 
span = langfuse.start_span(name="my-span")
 
span.end()
```

## Evaluarea Agentului

Observabilitatea ne oferă metrici, dar evaluarea este procesul de analizare a acestor date (și de efectuare a testelor) pentru a determina cât de bine performează un agent AI și cum poate fi îmbunătățit. Cu alte cuvinte, odată ce ai traseele și metricile, cum le folosești pentru a judeca agentul și a lua decizii?

Evaluarea regulată este importantă deoarece agenții AI sunt adesea nedeterministici și pot evolua (prin actualizări sau comportament de model în derivă) – fără evaluare, nu ai ști dacă "agentul inteligent" își face treaba bine sau dacă a regresat.

Există două categorii de evaluări pentru agenții AI: **evaluare online** și **evaluare offline**. Ambele sunt valoroase și se completează reciproc. De obicei, începem cu evaluarea offline, deoarece aceasta este etapa minimă necesară înainte de implementarea unui agent.

### Evaluare Offline

![Elemente de set de date în Langfuse](https://langfuse.com/images/cookbook/example-autogen-evaluation/example-dataset.png)

Aceasta implică evaluarea agentului într-un mediu controlat, de obicei folosind seturi de date de testare, nu interogări reale ale utilizatorilor. Folosești seturi de date curate în care știi care este rezultatul așteptat sau comportamentul corect și apoi rulezi agentul pe acestea.

De exemplu, dacă ai construit un agent pentru probleme de matematică, ai putea avea un [set de date de testare](https://huggingface.co/datasets/gsm8k) cu 100 de probleme cu răspunsuri cunoscute. Evaluarea offline se face adesea în timpul dezvoltării (și poate face parte din pipeline-urile CI/CD) pentru a verifica îmbunătățirile sau pentru a preveni regresiile. Beneficiul este că este **repetabil și poți obține metrici clare de acuratețe, deoarece ai adevărul de bază**. De asemenea, poți simula interogările utilizatorilor și măsura răspunsurile agentului în raport cu răspunsurile ideale sau poți utiliza metrici automate, așa cum s-a descris mai sus.

Provocarea cheie cu evaluarea offline este asigurarea că setul de date de testare este cuprinzător și rămâne relevant – agentul poate performa bine pe un set de testare fix, dar întâlni interogări foarte diferite în producție. Prin urmare, ar trebui să actualizezi seturile de testare cu noi cazuri limită și exemple care reflectă scenarii din lumea reală. Un mix de cazuri mici de "test rapid" și seturi de evaluare mai mari este util: seturi mici pentru verificări rapide și seturi mai mari pentru metrici de performanță mai largi.

### Evaluare Online

![Prezentare generală a metricilor de observabilitate](https://langfuse.com/images/cookbook/example-autogen-evaluation/dashboard.png)

Aceasta se referă la evaluarea agentului într-un mediu real, adică în timpul utilizării efective în producție. Evaluarea online implică monitorizarea performanței agentului în interacțiunile reale ale utilizatorilor și analiza continuă a rezultatelor.

De exemplu, ai putea urmări ratele de succes, scorurile de satisfacție ale utilizatorilor sau alte metrici pe traficul live. Avantajul evaluării online este că **captură lucruri pe care s-ar putea să nu le anticipezi într-un mediu de laborator** – poți observa deriva modelului în timp (dacă eficiența agentului se degradează pe măsură ce modelele de intrare se schimbă) și poți detecta interogări sau situații neașteptate care nu erau în datele tale de testare. Oferă o imagine reală a modului în care agentul se comportă în mediul real.

Evaluarea online implică adesea colectarea feedback-ului implicit și explicit al utilizatorilor, așa cum s-a discutat, și posibil rularea testelor de umbră sau testelor A/B (unde o versiune nouă a agentului rulează în paralel pentru a fi comparată cu cea veche). Provocarea este că poate fi dificil să obții etichete sau scoruri fiabile pentru interacțiunile live – s-ar putea să te bazezi pe feedback-ul utilizatorilor sau pe metrici din aval (cum ar fi dacă utilizatorul a dat clic pe rezultat).

### Combinarea celor două

Evaluările online și offline nu sunt exclusiviste; ele se completează reciproc. Informațiile din monitorizarea online (de exemplu, noi tipuri de interogări ale utilizatorilor unde agentul performează slab) pot fi utilizate pentru a îmbunătăți seturile de date de testare offline. În mod similar, agenții care performează bine în testele offline pot fi implementați și monitorizați online cu mai multă încredere.

De fapt, multe echipe adoptă o buclă:

_evaluează offline -> implementează -> monitorizează online -> colectează noi cazuri de eșec -> adaugă la setul de date offline -> rafinează agentul -> repetă_.

## Probleme Comune

Pe măsură ce implementezi agenți AI în producție, s-ar putea să întâlnești diverse provocări. Iată câteva probleme comune și soluțiile lor potențiale:

| **Problemă**    | **Soluție Potențială**   |
| ------------- | ------------------ |
| Agentul AI nu îndeplinește sarcinile în mod constant | - Rafinează promptul dat agentului AI; fii clar în privința obiectivelor.<br>- Identifică unde împărțirea sarcinilor în subtasks și gestionarea lor de către mai mulți agenți poate ajuta. |
| Agentul AI intră în bucle continue  | - Asigură-te că ai termeni și condiții clare de terminare, astfel încât agentul să știe când să oprească procesul.<br>- Pentru sarcini complexe care necesită raționament și planificare, folosește un model mai mare specializat pentru sarcini de raționament. |
| Apelurile de instrumente ale agentului AI nu performează bine   | - Testează și validează ieșirea instrumentului în afara sistemului agentului. |

- Rafinați parametrii definiți, prompturile și denumirea instrumentelor.  |
| Sistemul multi-agent nu funcționează constant | - Rafinați prompturile oferite fiecărui agent pentru a vă asigura că sunt specifice și distincte unul de celălalt.<br>- Construiți un sistem ierarhic folosind un agent "router" sau de control pentru a determina care agent este cel potrivit. |

Multe dintre aceste probleme pot fi identificate mai eficient dacă există observabilitate. Traseele și metricile discutate anterior ajută la identificarea exactă a locului în care apar problemele în fluxul de lucru al agentului, făcând procesul de depanare și optimizare mult mai eficient.

## Gestionarea costurilor

Iată câteva strategii pentru gestionarea costurilor asociate cu implementarea agenților AI în producție:

**Utilizarea modelelor mai mici:** Modelele de limbaj mici (SLM) pot performa bine în anumite cazuri de utilizare agentice și vor reduce semnificativ costurile. Așa cum s-a menționat anterior, construirea unui sistem de evaluare pentru a determina și compara performanța față de modelele mai mari este cea mai bună metodă de a înțelege cât de bine va performa un SLM pentru cazul dvs. de utilizare. Luați în considerare utilizarea SLM-urilor pentru sarcini mai simple, cum ar fi clasificarea intențiilor sau extragerea parametrilor, rezervând modelele mai mari pentru raționamente complexe.

**Utilizarea unui model router:** O strategie similară este utilizarea unei diversități de modele și dimensiuni. Puteți folosi un LLM/SLM sau o funcție serverless pentru a direcționa cererile în funcție de complexitate către modelele cele mai potrivite. Acest lucru va ajuta la reducerea costurilor, asigurând în același timp performanța pe sarcinile potrivite. De exemplu, direcționați interogările simple către modele mai mici și mai rapide și utilizați doar modelele mari și costisitoare pentru sarcini de raționament complex.

**Cache-ul răspunsurilor:** Identificarea cererilor și sarcinilor comune și furnizarea răspunsurilor înainte ca acestea să treacă prin sistemul agentic este o metodă bună de a reduce volumul cererilor similare. Puteți chiar implementa un flux pentru a identifica cât de similară este o cerere față de cererile cache-uite folosind modele AI mai de bază. Această strategie poate reduce semnificativ costurile pentru întrebările frecvente sau fluxurile de lucru comune.

## Să vedem cum funcționează în practică

În [notebook-ul exemplu al acestei secțiuni](../../../10-ai-agents-production/code_samples/10_autogen_evaluation.ipynb), vom vedea exemple despre cum putem folosi instrumentele de observabilitate pentru a monitoriza și evalua agentul nostru.

## Lecția anterioară

[Modelul de design Metacognition](../09-metacognition/README.md)

## Lecția următoare

[MCP](../11-mcp/README.md)

**Declinare de responsabilitate**:  
Acest document a fost tradus folosind serviciul de traducere AI [Co-op Translator](https://github.com/Azure/co-op-translator). Deși ne străduim să asigurăm acuratețea, vă rugăm să fiți conștienți că traducerile automate pot conține erori sau inexactități. Documentul original în limba sa natală ar trebui considerat sursa autoritară. Pentru informații critice, se recomandă traducerea profesională realizată de un specialist uman. Nu ne asumăm responsabilitatea pentru eventualele neînțelegeri sau interpretări greșite care pot apărea din utilizarea acestei traduceri.