<h1>W1: Snak med Dimitrios om at afgrænse emnet. Virker urealistisk stort, og du har ikke tid til det</h1>

<h2>Vedr. projektbeskrivelsen</h2> 
<li>Læg vægt på, at der i projektplanen skal indsættes noget f.eks. "examine the possibility of using this type of data with a ViT", i stedet for "implement a ViT"</li>
<li>Det virker meget ambitiøst, og bør nok nedskaleres</li>
Kristian kan se det ske, hvis han har enten:
<ul>
<li>Datasættet med segmentering også</li>
<li>Noget boiler-kode som du kan bruge </li>
</ul>

<h2>Vedr. Gantt-chart</h2>
<li>Brug dit Gantt-chart til at vise Dimitrios at det er fuldstændigt usandsynligt</li>
<li>Indsæt tid til toy-model i Gantt - det er fair, for der er ingen på BA niveau som har brugt det før. Det bør være en del af din projektformulering</li>
<li>Find en lærebog om transformere. Ikke artikler - det er for tidligt. </li>
<li> At læse om fundamentale DL-koncepter, og at øve dig i fundamentale DL-ting bør indgå i Gantt</li>
<h2>Vedr. selve rapporten og gennemlæsning af afsnit </h2>
<li>Lav relativt hurtigt et groft dokument med samtlige overskrifter for projektet og send til Dimitrios. Så bruger I ikke tid på det du allerede ved, og han ved hvad du har tænkt dig</li>
<li>T-3 Uger: Send resultatafsnit *SENEST*. Der skal være tid til at kunne lave det hele om</li>
<li>Send løbende chunks af kapitler. Det er nødvendigt at være på "forkant" med det nemme, således at du igen har tid til resultat-afsnittet. Det er der, hvor det først bliver kompliceret</li>


<h1>W2: lidt mere retning</h!>
![[network-sketch.jpg]]

Det til højre hedder en latent space repræsentation. Den skal du finde.

Opgaver til næste: 
- Læse videre
- Vælge to klasser, f.eks. fly og cykel. 
	- Lav histogrammer over valgte klasse
	- Undersøg om billederne kommer fra samme fordeling
	- Find et tal, som kan kvantificere afstanden til den mest gængse fordeling i datasættet
	- FJERN OUTLIERS (svære billeder)
	- Definer test-træn-split herefter
- Implementer center-cropping eller resizing i en fornuftig pipeline, således at den er klar og færdig.
	- Minimer antallet af "tabte" fikseringspunkter

- Snak med Dimitrios; 
	- Jeg ser det her som værende muligt hvis og kun hvis
		- Jeg er doven med latent space repræsentation, og derfor bare vælger en plug-n-play transformer
	- Han er villig til at sætte lidt tid af til at hjælpe med at designe selve transformer-arkitekturen
	- Det er et proof-of-concept, dvs. virker ikke for alle instances, men for valgte klasser 
	- Dvs. projektet handler om, hvorvidt det kan lade sig gøre, og ikke en "færdig" løsning
	- Evt. senere syntese-projekt 



<h1> d. 27/09/2022 </h1>
Projektplan: nogle steder: for konkret
Tre overordnede objectives, som skal indgå i rapporten. 
Objectives er i en artikel det "the goal of this study" etc. 
Concrete mål-definitioner når rapporten er ved at blive skrevet: "Er min vejleder med på det". 

Der er stor fare for det med at stå i kø i HPC 
	- Køre dine modeller colab eller interaktivt. 
	- Check at du kan overfitte groft og at modellen konvergerer

Du kan godt tænke over det helt til sidst - ifht. EDA, flybillede, svært punkt
Sikrer dig at have de åndssvage billeder i dit testset, ellers kan du ikke se "at de er svære"

Test-træningssplit: 
	- shuffle meget
	- 80/20 split - med seeding
		- Sikrer dig at fordelingerne ser nogenlunde ens ud
			- Valideringssplit laves tilfældigt under træning 

Baseline-model: 
	- Kristian synes ikke at det giver vildt meget mening ifht. en reel baseline. Men det er en fin øvelse, formentlig
	- Den dårligste/mest simple af Dimitrios' modeller 
	- Spørgsmål til Dimitrios: hvor meget renggører du data? Ellers er det en unfair sammenligning 




Tjek: 
- Normalisering af dataen. Er alle pixel-intensiteter mellem 0 og 255? 


To-do: 
- Inden næste uge:
	- Sæt låg på dataprocessering nu   - gjort 
	- Etabler et seeded, tilfredsstillende test/træningssplit
	- Tjek at dataen er normaliseret i [0,1] - brug KUN parametre fra træningssæt. Eller [0,255] inden du bruger TIMM. 



	- Begynd at bygge en simpel transformer - se hvor lang tid den tager at træne!
	- Begynd at skrive teori-afsnit til transformeren
	- Hvad er grunden til ikke bare at bruge en vanilla ViT? 
		- Hvorfor antager du at vi skal bruge en latent-space model? 
	- Tag tid 

Brug _Rbbfix.txt_ som træningssæt og rfix som testsæt


Q: Ways to overfit 
Lille cut af træningsdata 
Standard decay på LR 
Kør epoker indtil træningfejl 0 og valideringsfejl uendelig

To-do: 
	- Lav skelet til rapport
	- Mindst 30-35 sider 
		- Kristian foreslår ca. 7 siders teori 
		- 5(evt. + Siders metode
		- 10 Siders analyse
	- Formidling til: dig selv før du begyndte på projektet 
	- Forstår vision transformer 

D_model: skriv tingene ned med det samme.  


Hvordan definerer du et netværk, hvor batch-size går 
- flatten batch-wise 
- check om lineaært lag fatter at batchsize er variabel

Activation-function: 
	- Gå activations igennem. Se om der er mange 0'er 
	- Check værdierne lige efter laget med aktivering 
	- Dead-relu? 
	- hvis ja: 
		- leaky-relu med bitte alpha-værdi til en start . 0.001, 0.005, 0.01~
	- Gradient explosion er nok lidt usandsynligt 
	- Fanget i minimum? 
	- Learning-rate meget ned samtidigt m. batch-size

- Loss-F: 
	- Nogle bruger RMSE eller MSE  til bbox i DL (ikke nødvendigvis optimalt, men nemt)
	- ((sqrt(x_t)- sqrt(x_t))ˆ2 + (sqrt(y_true)- sqrt(y_pred))ˆ2)
	- YOLO: center, h, w (for bbox)
		- ((sqrt(xc_t)- sqrt(xc_p))ˆ2 + (sqrt(yc_true)- sqrt(yc_pred))ˆ2)
		med 
		 ((sqrt(h_t)- sqrt(h_p))ˆ2 + (sqrt(w_true)- sqrt(w_pred))ˆ2)
- CrossEntropy er til fordelinger 
	- Kun med fordelingsfkt. som sidste lag (eller andre tilfælde end det du er i nu)



Alt + space -> lg -> enter  -> global.context.unsafe_mode = true


Introduktionsafsnit: 
- Aim, målafgrænsning
- Forslag til delafsnit i introduktion: 
	- Struktur af rapporten afsnit
	- Aim and or objectives
		- Aim: sagprosa, objectives er bullets
		- Objectives er derved en højere opløsning af aim-afgrænsningen
		- Scope-afsnit (motivation for emnet)
			- Hvis Dimitrios prøver at løse noget konkret 
- Afsnit 1.2) er farligt, for det kan godt minde lidt for meget om en tekstbog. 
	- Ingen undersektioner per trin, mere som en sammenhængende historie. 
	- Du skal forklare masking i brødteksten, ikke som et underafsnit f.eks. 
	- Undersektion til elementer/komponenter 

Overvej at smide transformer-teori i et teori-afsnit i stedet for i introduktionen. 

Resultater minder meget om analyse i din rapport. Overvej at slå dem sammen og diskuter resultater i resultatafsnit.   

Diskussion: 
implikation, applikation, validitet 


Retningslinje: 
- Forside regler 
- IEEE referenceformat.
- Forfatter & år når man refererer - det er rarest for folk i feltet. 
	- (forfatter, år)
	- "\citep{}"" i overleaf
	- "masking is when you mask something" (artikel, år)
	- "As stated in Attention is All You Need" 
	- Lærebog er cool 


Prøv med lavere hidden-dimension
Få torchsummary af model med tidligere størrelser og se antallet af parametre 




