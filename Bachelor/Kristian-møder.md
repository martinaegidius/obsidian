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




<h1> Møde d. 26/10/22 </h1> 
torch.std(norm(x),dim=(1,2)) -> sådan tjekker du om mean af batch er 0 og std er 1 
Plot lige preds og target oven på overfit-billederne. 

IOU- for "god" detection ofte 0.5-0.7 (i litteraturen)

PAC på 4 billeder atm: 0.47334410339256866: Kristian siger ret dårligt :-) 

Kig på billederne som du overfitter på - og generelt på sættet for fly. Det kan godt være, at flyene som udgangspunkt bare fylder nærmest hele billedet -> nemt at få IOU 

Til at tjekke efter:
Lille lol-model som infererer box ud fra 1. gennemsnit 2. median-værdi af box-hjørner 
- Hvis halvdelen af billederne eller flere har box som fylder hele billedet, så vil lol-model få PAC på ca. 0.5 -> og så er din model altså dårligere ligenu 
- Den af de to lol-modeller, som performer bedst bruger du som "baseline til din baseline" 
- Husk æbler og æbler 

Cross-validation kan nok ikke undgås :(((( 
Lille valideringssæt, 10%. K-fold på 10 eller noget i den dur
- Mest for at tjekke om du overfitter
- Du har så få samples -> ~generalization


Kristians pointer: 
- Nogle plots (træningsloss, valloss, baseline) (milestones)
- Forstå træningsklassen 
- Overleaf: milestone om 2-3 uger. 
	- Subfiles 
	- Der har du lavet første del af introduktionen færdig 
	- Teorien om transformere færdige 

Studieplan: 
- Earth observation (specialisering)
- https://www.dtu.dk/english/education/graduate/msc-programmes/earth-and-space-physics-and-engineering/study-lines/earth-observation
- Send dit fokusområde til Kristian. Send excel-ark med dine SKAL fag
	- Find kurser både til fordybelse og nogle som er "nemme"

TIL NÆSTE GANG: 
Fokus: få styr på de andre ting, som generer. 
- Studieplan 
- Send til  
- Skriv til SU
- Reflektion over "spørgeskemaet"
- Lette ting i forhold til Bachelor
	- Plots over dine fire billeder 
	- Plots over dine ti billeder
	- Baseline til baseline model
	- Evt. begynd at skriv på afsnittet med metode 

Martin nævner: 
- Machine Learning Operations
	- MLOps


8. Semester virker lidt hæftigt 
	- Videregående billedanalyse er et ret hæftigt kursus (meget implementering, hvis man skal få udbytte) 
	- Mange af kurserne virker som om at man laver mere end man skal på det her semester 
	- Måske kan det godt lade sig gøre hvis du beslutter dig for at vælge et af de nemmeste innovationskurser (analytics to action virker også ret nemt)

Overvej at rykke dyb læring tidligt - dvs. 9. semester 

Skriv til KU om regler 

Learning-rate: prøv at lave den mindre 

- Smid median-IOU og mean-IOU ind i tabel 
- Plot - kun halvdelen 
- Resultater ned i resultat-afsnit 
- Træningskurver 
- Kun en i rapporten - rest i appendix 
- Helt færdig med alle baselines



#todo 



Generer alle plots igen og smid dem ind i rapporten. Baseline er herved færdig
- ca. 5 arbejdstimer - baseline resultater færdige og skrevet ind. Ikke afsnit nødv.
	- Ny baseline 6,3. 
	- Status: færdig
	- Logfiles: results/NL_6_NH_2_final_results
	- Graphs: 1_fold_results_nl_6_nh_2. Alle resultater for modeller FØR early-stopping 
	- Models: in NL_6_NH_2_final_results/fullRun


- Load KUN layer 1 - hvor mange du skal bruge i første omgang _> hurtigere'
- vit-del: 15-20 timer bare for at få det til at køre 
	- FOR ET BILLEDE
- Lav din gamle model om, så du kan smide latent space ind. [10-15t] 
	- RESULTATERNE SKAL IKKE VÆRE GODE 
	- HVIS DU BLIVER FÆRDIG FØR TID: PRØV AT OVERFITTE LØBENDE FOR LÆNGERE SÆT 
- Metode til det du laver (simultant). ~3 siders mere metode. 

 

Overfitting-del skal fylde mindre 
Resultater over i resultater

Tilmelding til kurser KU 

Make sure that patchify is rowwise or column-wise. Use a very simple images black triangle. 
Dim kommentar: 
1 feature-representation per patch -> linear-interpolate. 


Atm status: You have made a custom load_POET_timm. When running timm

[Errno 2] No such file or directory: '/zhome/ba/f/147212/BA/timm/../eyeFormer//../Data/POETdataset/etData/etData_aeroplane.mat'

Figure out where the extra /../ after eyeFormer comes from. 
