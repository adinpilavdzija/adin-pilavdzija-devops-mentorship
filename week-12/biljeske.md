Architecture Deep Dive

-Monolitna arhitektura kao crna kutija sa svim komponentama aplikacije u njoj. Komponente: Upload, Processing, Store and Manage... Odlike monolitne arhitekture:
Sve je jedan entitet, pa ako jedan dio ne radi ispravno onda citav entitet ne radi ispravno. 
Kada monolit skalira, skaliraju sve njegove komponente. Skalira se vertikalno jer sve komponente ocekuju da su pokrenutu na istom compute hardwareu.
Sve komponente prave troskove, cak i ako se ne koriste

-Tiered architecture
Razdvojena komponente monolitne strukture na tierove, gdje svaki tier moze biti na istom ili razlicitom serveru.
Razlicite komponente su povezane (coupled together) jer je svaki tier povezan s endpointom drugog tiera
Svaki pojedini tier moze zasebno vertikalno skalirati
Tierovi nisu povezani direktno s drugim tierovima (endpoint), nego se koristi Load Balancer sto omogucava i horizontalno skaliranje (dodatne instance) uz sto postaje visoko dostupno.
Mane: tierovi su idalje sastavljeni (npr Upload ocekuje i zahtijeva da Processing postoji i odgovori)
Nemogucnost skaliranja pojedinog tiera na 0 jer je komunikacija sinhronizirana

-Tiered architecture mozemo nadograditi koristeci Queues
Queue je sistem koji prihvata messsages.
Poruke iz Queuea su cesto primljene First In First Out arhitekturi.
Poruke sadrze podatke vezane za posao koji treba obaviti i metadata (npr. upload slike na S3, slika se salje na S3 a poruka sa metapodacima se salje u red i ceka da je odgovarajuci servis preuzme (asinhrona komunikacija))
Auto Scaling Group detektuje velicinu reda i kreira potrebne (ili terminira nepotrebne) instance koje preuzimaju poruke iz queuea
Queue izmedju dva aplikacijska tiera decouples (razdvaja) te tierove
Komponente mogu skalirati nezavisno jedna od druge, od 0 do priblizno beskonacno, u zavisnosti jedino od velicine reda

Microservice Architecture
Monolitna aplikacija razdvojena u jako male dijelove tj mikroservise
Mikroservisi rade jednu stvar jako dobro. Oni su mala, samodovoljna aplikacija.
Upload service je producer (proizvode data ili poruke), Processing je consumer (koriste  data ili poruke), a Store and Manage oboje. 
Servisi proizvode i koriste evente.

Event-Driven Architecture
Event-Driven Architecture zasniva se na eventima, bilo da se radi o servisima koji ih generisu (Event producer) ili servisima koji se pokrecu kada se generise neki event i dodje do njih (Event consumers)
Ni produceri ni consumeri ne cekaju da se nesto desi, ne trose konstantno resurse. Kod producera, eventi su generisani kad se nesto desi (npr button click), a consumeri primaju te evente i prestaju s radom po zavrsetku
Najbolja praksa za Event-Driven Architecture je koristenje Event Routera (visoko dostupnog centralnog exchange pointa za evente). Event Router ima Event Bus (kao konstantan protok informacija). 
Event Producer generise eventi, oni idu u event bus i router ih dodijeli event consumeru
Resursi se trose samo kad je potrebno (jedna od kljucnih komponenti serverless arhitekture)
________________________


AWS Lambda - PART1
-Lambda je Function-as-a-Service (FaaS)
-proslijedimo joj kratak i specijaliziran kod, odradi to i naplati samo ono sto se potrosi
-Lambda funkcija je dio koda koji Lambda pokrece
-Funkcije se ucitavaju i izvrsavaju u runtime environmentu
-Kod kreiranja Lambda funkcije definisemo i kolicinu memorije (indirektno se alocira kolicina CPU). Kolicina memorije i vCPU direktno proporcionalni
-Naplacuje se samo vrijeme izvrsavanja funkcije
-Lambda je kljucan dio Serverless arhitekture
-Lambda funkcija = kod + pridruzeni wrapping i konfiguracija
-Docker je anti-pattern za Lambdu
-Lambda podrzava koristenje Docker imagea i drugih imagea, ali u smislu da su to build procesi
-Lambda funkcije su stateless, sto znaci da nije ostala data od ranijih poziva, svaki poziv je novo okruzenje
-Svaki put kada se Lambda funkcija pozove na izvrsenje, kreira se novi runtime enviroment sa svim potrebnim komponentama koje Lambda funkcija zahtjeva za adekvatno izvrsenje. Kod se ucita, izvrsi i terminira
-Alokacija memorije od 128MB do 1024MB sa 1MB koracima
-Function timeout = 15min 

Nacini na koje se Lambda cesto koristi:
-Serverless Applications (S3, API Gateway, Lambda)
-File Processing (S3, S3 Events, Lambda)
-Database triggers (DynamoDB, Streams, Lambda)
-Serverless CRON (EventBridge/CWEvents + Lambda)
-Realtime Stream Data Processing (Kinesis + Lambda)

2
-Lambda ima 2 networking modea: public (default) i VPC networking

PUBLIC
-Defaultno Lambda funkcije su u public networking mode, cime je omogucena komunikacija sa public AWS servisima i public Internetom
(ako je i pristup servisima takodje dozvoljen pomocu authentication i authorization metoda)
-Lambda funkcije nemaju pristup servisima unutar nekog VPC-a ukoliko nema njihove javne IP adrese i security pravila ne dozvoljavaju eksterni pristup

VPC mode
-Lambda konfigurisana unutar privatnog subneta u VPC-u
-Sva pravila koja unutar VPC-a vrijede za druge servise vrijede i za Lambdu
-Lambda moze pristupiti drugim servisima unutar VPC-a ukoliko to dozvoljavaju NACL i Security Groups, ali ne moze pristupiti servisima izvan VPC-a ukoliko nije drugacije konfigurisano na nivou VPC-a
-za izlaz van VPC-a mozemo koristiti VPC Endpoint (za pristup public AWS servisima), za izlaz na internet, koristimo NAT gateway + Internet Gateway

NEW WAY: AWS analizira sve funkcije koje su pokrenute unutar regiona tj. unutar account-a, te pravi jedinstvene kombinacije subneta i security grupa. Za svaku jedinstvenu kombinaciju, zahtjeva se 1 ENI unutar VPC-a.

-Kako bi runtime enviroment imao pristup AWS proizvodima i servisima mora imati Execution role (zakacenu za nasu Lambda funkciju)
-2 dijela Lambda permissions:
1. Roles permission policy, (kao EC2 instance role) koji se odnosi na kod koji Lambda pokrece, a koji preuzima od Lambda funkcije premisije definisane za tu rolu. Ovim se kreiraju privremeni kredencijali koje Lambda funkcija koristi za pristup servisima 
2. Trust policy, koji vjeruje Lambda funkciji
-Lambda resource policy nesto kao S3 bucket policy. Lambda resource policy kontrolise koji servisi i accounti mogu pozivati lambda funkcije 
-Resource policy se moze rucno mijenjati kroz CLI ili API

-Lambda za logging i monitoring koristi CloudWatch, CloudWatch Logs i X-Ray
-Kako bi Lambda mogla da cuva logove unutar CloudWatch Logs mora imati permisije definisane unutar Execution role



3
-3 metode pozivanja Lambda funkcije:
Synchronous invocation
Asynchronous invocation
Invocation using Event Source mappings


Synchronous invocation
-Kroz CLI ili API pozivamo Lambda funkciju. Lambda funkcija prima neke podatke i izvrsava, dok CLI ili API ceka odgovor jer je sinhrono. Kada Lambda funkcija zavrsi, vraca odgovor.
-Synchronous invocation se desava i ako se Lambda koristi indirektno preko API Gatewaya (use-case za mnoge Serverless arhitekture)
SLIKA1
-Synchronous invocation koristi se kada covjek tj. klijent direktno utice na invoking Lambda funkcije.

Asynchronous invocation
-Obicno se koristi kada AWS servisi pozivaju Lambda funkciju
-idempotent operation - koliko god puta da se pokrene, ishod ce biti isti (npr postavljanje balancea na 20$)
-Eventovi mogu biti poslani u Dead Letter Queue nakon nekoliko bezuspjesnih pokusaja
-Lambda ima mogucnost kreiranja destinacija
SLIKA2

Event Source Mapping
-uglavnom se koristi za Streams ili Queues koji nemaju mogucnost generisanja eventa koji bi trigerovali pokretanje Lambda funkcije (Kinesis, DynamoDB streams, SQS) - eventi kod koji zahtjevaju polling tj. povlacenje podataka iz reda
-Kinesis Data stream je Stream-based proizvod, koji omogucava korisnicima da citaju iz Stream-a, ali nema generisanja dogadjaja kada se doda novi podatak u Stream
-Kako bismo koristili Kinesis Data stream u kombinaciji sa Lambda funkcijom potreban nam je Event Source mapping servis koji radi polling podataka iz Stream-a, trazi nove podatke i vraca nazad Source batches. Batch podaci su podijeljeni po velicini batch-a i poslati Lambda fuknciji kao Event batches
-Kod Asinhronih poziva Lambda funkcije, nisu potrebne permisije za pristup izvornom servisu jer on samostalno triggeruje izvrsenje Lambda funkcije, osim u slucaju kada je potrebno preuzeti jos informacija od izvornog servisa
-Pri koristenju Event Source mapping izvorni servis ne radi direktni invoke Lambda funkcije. ES mapping cita podatke iz izvornog servisa koristeci Lambda Execution role permisije da bi pristupio tim podacima. Tako da se ES mapping koristi samo kao posrednik u ime Lambda funkcije i koristi njene permisije iz Execution roles kako bi prikupio potrebne podatke

-Lambda funkcije imaju verzije (verzija je kod i konfiguracija)
-Verzija je immutable
-$Latest pokazuje na posljednju/najnoviju verziju Lambda funkcije i mijenja se u skladu sa objavljivanjem novih verzija (not-immutable)
-Alias (DEV, STAGE, PROD)

-Cold start je potpuno kreiranje i konfigurisanje okruzenja (~100ms)
-Warm start se desava kada se ista Lambda funkcija pozove opet bez previse vremenskog razmaka i koristi isti execution context, bez potrebe konfigurisanja (~1-2ms)
-ako prodje previse vremena izmedju dva poziva, context ce biti izbrisan sto dovodi opet do cold starta
-1 execution context = 1 invocation (20 poziva Lambda funkcije = 20 cold startova)
-Provisioned concurrency - za high load poziva Lambda funkcije
-Moguce je i koristiti /tmp unutar execution contexta pa ukoliko druga funkcija koristi isti context, imat ce pristup tim fajlovima
-Sve sto se definise unutar Lambda function handler-a postoji samo u toku te invokacije Lamda funkcije. Sve sto zelimo opet koristiti kasnije mozemo kreirati van Lambda function handlera
___________________

CloudWatchEvents and EventBridge
-CloudWatchEvents dostavljaju near real-time stream of system events koji su se desili nekim od AWS servisa
-EventBridge je servis koji zamjenjuje CloudWatchEvents, ima sve iste funkcionalnosti i dodatno moze handleati events od third parties kao i custom applications
-CloudWatchEvents i EventBridge omogucavaju implementaciju arhitekture koja moze pratiti "if X happens, or at Y time(s) ... do Z"
-EventBridge = CloudWatchEvents v2
-bolje koristiti EventBridge 
-Oba servisa imaju istu baznu arhitekturu, koriste default entity - Event Bus, te imaju default event bus za AWS account
-Bus je stream evenata koji se desavaju od bilo kojeg AWS servisa u jednom accountu
-U CWE postoji samo 1 event bus, dok u EB mozemo kreirati dodatne
-Event Pattern rule i Schedule rule
-Eventi su samo JSON strukture
______________________

[ASSOCIATESHARED] [DEMO] Automated EC2 Control using Lambda and Events - PART1 DEMO
-Execution role je AWS service role

____________

Serverless Architecture
-Serverless isn't one single thing
-U serverlessu razbijamo aplikaciju u sitne dijelove, cak i manje od mikroservisa
-Applications are a collection of small & specialised functions
-Funkcije koje cine aplikaciju se vrte u Stateless i Ephemeral okruzenjima 
-Lambda je jeftina jer je scalable
-Sve je event-driven, nista ne koristi resurse dok nije potrebno
-Serverless okruzenje treba koristiti gdje je moguce managed servise
-Limit za broj IAM usera unutar jednog AWS accounta = 5000 IAM usera
-Cognito u ovom slucaju mijenja Google token za AWS Temp Credentials jer AWS ne moze direktno koristiti third-party identities
-Elastic Transcoder - servis koji prima media i moze vrsiti manipulacije
-

SLIKA SERVERLESS
_____________

Simple Notification Service
-SNS je highly available, durable, secure, pub/sub messaging service
-SNS je public AWS servis sto znaci da za pristup trebamo network connectivity sa public AWS endpointom. Benefit je da je dostupan gdje god ima network connectivity
-koordinira slanje i dostavljanje poruka
-poruka = 256KB payloads
-Bazni entitet SNS su SNS Topici gdje se kontrolisu permisije i gdje je vecina konfiguracije podesena
-publisher - sends messages to a TOPIC
-Topici imaju i Subscribere koji po defaultu primaju sve poruke koje se salju u topic
-Subscriberi: HTTP(s), Email, SQS, Mobile Push, SMS Messages i Lambda
-CloudWatch i CloudFront koriste SNS
-SNS radi u AWS Public Zone i moze mu se pristupiti iz Public Interneta ako entitet koji pokusava pristupiti ima adekvatne permisije. Moze se pristupiti i iz VPC-a ako je VPC podesen da moze pristupiti javnim endpointima
-Stvari mogu biti Producer i Subscriber u isto vrijeme (npr APIji)
-moguce je napraviti filter za subscribere da dobijaju samo relevantne poruke
-Fanout - kad imamo jedan SNS topic sa vise SQS Queues u ulozi subscribera
-SNS nudi: Delivery Status (including HTTPS, Lambda, SQS), 
Delivery Retries (reliable delivery), High Availability and Scalable (Regionally resilient service), Server Side Encryption (any data which needs to be stored persistently on disc can be done so in an encrypted form), Cross-Account via TOPIC Policy (kao sto moze i S3)
_____________

Step Functions
-Step Functions address some of the limitations (desing decisions) with Lambda
-Nikad ne bi trebali pokusati citavu aplikaciju staviti unutar Lambda funkcije
-Lambda ima 15-min maksimalni execution time
-Mozemo povezati vise Lambda funkcija (kraj prve poziva sljedecu funkciju), ali je problematican pristup za skaliranje
-Runtime Enviroments are stateless 
-Step Functions je servis koji nam omogucava kreiranje State masina
-State masina = Start -> States -> End
-States uzimaju data, obradjuju data, vracaju data
-Standard Workflow je default i ima 1 godinu execution limit
-Express Workflow je dizajniran za high volume event-processing workloads npr. IOT, streaming, data processing itd. - run up to 5 min
-Koristimo standard za long-running, express za highly transactional i traze vise processing guarantees
-mozemo kreirati template za kreiranje i export SM-a koji se zove - Amazon States Language (ASL) koji je baziran na json-u
-permisije State masinama se dodjeljuju pomocu IAM roles
-States are things inside the workflow, things which occur
-Stanja: Succeed & fail, Wait, Choice, Parallel, Map, Task
________________

API Gateway 101
-API Gateway je servis koji nam omogucava kreiranje i manage API-ja. 
-API predstavlja nacin na koji aplikacije medjusobno komuniciraju
-API Gateway sluzi kao endpoint ili entry-point za aplikacije koje pokusavaju komunicirati sa nasim servisima
-poveznica je izmedju aplikacija i integracija (servisa)
-highly available, scalable, handles authorisation, throttling, caching, CORS, transformations, OpenAPI spec, direct integration and much more
-API Gateway je javni servis i moze djelovati kao front end za servise koji su pokrenuti unutar AWS ili on-premises
-HTTP APIsm REST APIs and WebSocket APIs
-API Gateway is an intermediary between clients and integrations
-3 faze API Gateway interakcija: 
1. Request - klijent pravi request prema API Gatewayu (Authorize, Validate, Transform)
2. Request ide kroz API Gateway do servisa provideanih od strane integracija
3. Response - odgovor se dostavlja klijentu (Transform, Prepare, Return)

-API Gateway podrzava razlicite authentication metode
-moze koristiti Cognito user pool za autentifikaciju (jedna od metoda)
-Lambda based authorization (custom authorization)
-IAM se moze koristiti za autentifikaciju i autorizaciju sa API Gateway-om prosljedjivanjem kredencijala u headerima
-Tipovi endpointa: Edge-optimized, 
Regional, Private
-Kada deployamo API Configuration u API Gateway, to se desava u stage
-npr. prod i dev stage
-Vecina stvari u API Gatewayu su definisane na osnovu stagea
-Svaki stage ima svoj endpoint URL kao i postavke
-Mozemo omoguciti canary deployment (sub part of the stage) na stageove. Na njima mozemo konfigurisati da odredjeni dio saobracaja ide na canary. Canary mozemo i promovisati u new base stage
-Uvijek mozemo izbrisati canary i vratiti se na base stage

SLIKA ERRORI

-Caching je konfigurisan po stageu
-Bez cachea, svaki user aplikacije pravi request prema API Gateway stageu i tu su neke backend integracije/servisi koje opsluze taj request. Bez cachea, ti servisi bi se koristili za svaki request. Koristenje cachea znaci da ce pozivi biti prosljedjeni backendu kad je request = cache miss
-Cache moze biti enkriptovan
_______________

Build A Serverless App - Pet-Cuddle-o-Tron [DEMO]

______________

Simple Queue Service (SQS)
-SQS provides managed messaged queues
-Queues are highly available and highly performant by desing
-2 tipa queuea: Standard queues (poruke se mogu primiti drukcije od redoslijeda) i FIFO queues (garantuju redoslijed)
-Messages up to 256KB in size, link to large data
-Client send messages to queue, other clients poll the queue
-Polling - checking for any messages on a queue
-Kada klijenta poll-a i primi poruke, te poruke nisu izbrisane iz queuea, samo su izbrisane odredjeni period (VisibilityTimeout)
-VisibilityTimeout je kolicina vremena za koju klijent moze procesirati poruku na neki nacin
-Nakon procesiranja poruke, klijent je moze eksplicitno izbrisati iz queuea. Ako je ne izbrise eksplicitno, poruka ce se nakon VisibilityTimeouta ponovo pojaviti u queueu. Ovo je odlican nacin za osiguravanje fault tolerance
-Dead-Letter queues can be used for problem messages (npr za poruku koja je primljena 5 ili vise puta, a nije uspjesno obrisana)
-Dead-Letter queue omogucava razliciti set procesiranja na porukama koje mogu biti problematicne
-Queues can be used to decouple application components. One component add things to the queue, another reads from the queue. Neither component needs to be aware of worry about the other
-Queue je odlican i za skaliranje. ASG mogu skalirati na onovu velicine Queuea, a Lambda moze biti pozvana kada se poruke pojave u Queue

SLIKA FANOUT

-Standard kao autoput sa vise traka, FIFO kao put sa samo jednom trakom i bez mogucnosti preticanja
-Standard = at-least-once (bez garancije ocuvanja redoslijeda), FIFO = exactly-one (po tacnom redoslijedu)
-FIFO (Performance) 3000 messages per second with batching, or up to 300 messages per second without
-po pitanju SQS-a, naplacuju se requestovi (request nije isto sto i message)
-1 request = 1-10 messages up to 64KB total
-2 nacina SQS Queue Pollinga:
short (immediate) - koristi jedan request i moze primiti 0 ili vise poruka, ali ako Queue ima 0 poruka svejedno ce biti request koji ce vratiti 0 poruka
long (waitTimeSeconds) - vrijeme cekanja do 20s
-Long polling je preporuceni nacin
-Poruke mogu biti u SQS Queue do 14 dana. Podrzana je Encryption at rest (using KMS), a po defaultu data je enkriptovana in-transit
-Access to a queue is based on identity policies or we can also use Queue policy
-Queue polices can only allow access from external accounts. Queue policy is just a resource policy (like S3 bucket policy)
__________________

SQS Standard vs FIFO Queues
-FIFO mora imati sufiks .fifo da bi bio validan FIFO Queue

SLIKA standard vs fifo
_______________

SQS Delay Queues
-Delay Queues at a high level allow you to postpone the delivery of the messages to consumer
-With Delay queue we configure a value called 'DelaySeconds' on that queue which means that messages added to that queue wil start of in an invisible state for that period of time
-

SLIKA delay queues




