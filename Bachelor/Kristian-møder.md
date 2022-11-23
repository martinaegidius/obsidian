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


Status: deep-feature extraction complete. Implement transformer. Takes as input get_deep_seq output. Should implement CLS for xy. May be some sort of ragged implementation then.. 


Ring til læge for i morgen 

Random split sikrer ikke nødvendigvis at bokse er fra samme fordeling, størrelser er fra samme fordeling etc etc 
- Du kan vise mean-median-box på hhv træn val test og se at de er forskellige (formentlig). 

Næste uge: 
1. Skrive baseline helt færdig. Bum. 
~10 timer på at sammenknytte de to model-arme, og rette til, således at det kan træne til overfit. Hvis du ikke kan få den til at overfitte efter 4 timer så skid hul i det ;) 
- Gør det muligt at køre anden del uafhængigt af første del (gem output af første del og load)
~2-3 timer bagefter på at kigge lidt på nye resultater 

Hvis tid til overs: 

- Er resultaterne gode nok? 
	- Kan det give mening?
	- Hvis overfit ja: kør hele svinet. Hvis nej: få den til det 
	- Sæt til træning: falder train-loss? nej -> pil ved parametre. 
		- Når sat igang; hvordan kan du visualisere dine resultater?
		- Hvad skal du visualisere?
	- Prioriter at skrive i overleaf, hvis du kan overfitte. Ikke noget pille videre.



Status: Du har kørt git clean. Du har tabt /Results/, ligeledes /Data. 
De bør ligge på HPC, evt. klon dem over, hvis du får brug for dem igen


>Try beta three different values 
>NHEADS 
>NLAYERS
>IF WORKS: TRY GIOU 
>For EVALUATION-metrics: 
>	Consider stratifiying accuracy based on area of object 
>	Histogram of area of ground-truth boxes and make different splits 
>	cocpdataset.org/#detection-eval
>		Finetune area and look performance on small, medium, large 


1, 2, 6 layers
1, 2, 4 heads 

35-40% (Dim thinks intuitively would be performance)
ViT close to 50%
- Multiple annotators may improve performance a bit 


OBS: Flydataset 
train: 
mean-model: tensor([[0.1519, 0.2581, 0.8190, 0.7341]])
- Area: 0.318 square pixel percentage of image 
median-model: tensor([[0.1020, 0.2703, 0.8900, 0.7087]])

test:
mean-model: tensor([[0.1257, 0.2695, 0.8725, 0.7274]])
- Area: 0.342 square pixel percentage of image 
	- NOT DRAWN FROM SAME DISTRIBUTION
median-model: tensor([[0.0540, 0.2653, 0.9500, 0.7147]])




#todo: 
add to ViT_no_cmd 

#generate_DS: 
def generate_DS(dataFrame,classes=[x for x in range(10)]):
    """ Note: as implemented now works for two classes only. TODO: IMPLEMENT FOR VARIABLE LENGTH OF CLASSES
    Args
        classes: list of input-classes of interest {0:"aeroplane",1:"bicycle",2:"boat",3:"cat",4:"cow",5:"diningtable",6:"dog",7:"horse",8:"motorbike",9:"sofa"}   
        If none is provided, fetch all 9 classes
        
        returns: 
            A tensor which contains: 1. image-filenames, 2. bounding box, 3. class number, 4:8: a list of processed cleaned both-eye-fixations
            A list of filenames
            A tensor which contains all eyetracking data
            A tensor containing bounding boxes
            A tensor containing numerated classlabels
            A tensor containing image-dimensions
    """
    dataFrame.loadmat()
    dataFrame.convert_eyetracking_data(CLEANUP=True,STATS=True)
    NUMCLASSES = len(classes) #TODO: IMPLEMENT GENERATE_DS FOR MORE THAN TWO CLASSES
    
    
    dslen = 0
    dslen_li = []
    for CN in classes: 
        #print(CN)
        dslen += len(dataFrame.etData[CN])
        dslen_li.append(len(dataFrame.etData[CN]))
        #print("Total length is ",dslen)
        
    #print(dslen_li)
    
    df = np.empty((dslen,8),dtype=object)
    
    #1. fix object type by applying zero-padding
    num_points = 32
    eyes = torch.zeros((dslen,dataFrame.NUM_TRACKERS,num_points,2))
    #2.saving files to list
    filenames = []
    #3: saving target in its own array
    targets = np.empty((dslen,4))
    #classlabels and imdims to own array
    classlabels = np.empty((dslen))
    imdims = np.empty((dslen,2))
    
    
    
    datalist = []
    for CN in classes:
        datalist.append(dataFrame.etData[CN])
        #print("DSET {} LENGTH is: ".format(CN),len(dataFrame.etData[CN]))
    
    idx = 0
    IDXSHIFTER = 0
    k = 0
    for i in range(dslen): 
        if((i==sum(dslen_li[:k+1])) and (i!=0)): #evaluates to true when class-set k is depleted
            #print("depleted dataset {} at idx {}".format(k,i))
            IDXSHIFTER += dslen_li[k]
            k += 1
            
        idx = i - IDXSHIFTER
        df[i,0] = [dataFrame.etData[classes[k]][idx].filename+".jpg"]
        filenames.append(dataFrame.classes[classes[k]]+"_"+dataFrame.etData[classes[k]][idx].filename+".jpg")
        df[i,1] = dataFrame.etData[classes[k]][idx].gtbb
        df[i,2] = classes[k]
        targets[i] =  dataFrame.etData[classes[k]][idx].gtbb
        classlabels[i] = classes[k]
        imdims[i] = dataFrame.etData[classes[k]][idx].dimensions[:2] #some classes also have channel-dim
        for j in range(dataFrame.NUM_TRACKERS):
            sliceShape = dataFrame.eyeData[classes[k]][idx][j][0].shape
            df[i,3+j] = dataFrame.eyeData[classes[k]][idx][j][0] #list items
            if(sliceShape != (2,0)): #for all non-empty
                eyes[i,j,:sliceShape[0],:] = torch.from_numpy(dataFrame.eyeData[classes[k]][idx][j][0][-32:].astype(np.int32)) #some entries are in uint16, which torch does not support
            else: 
                eyes[i,j,:,:] = 0.0
                #print("error-filled measurement [entryNo,participantNo]: ",idx,",",j)
                #print(eyes[i,j])
            
            
            
    print("Total length is: ",dslen)
    
    targetsTensor = torch.from_numpy(targets)
    classlabelsTensor = torch.from_numpy(classlabels)
    imdimsTensor = torch.from_numpy(imdims)
    
    return df,filenames,eyes,targetsTensor,classlabelsTensor,imdimsTensor,dslen


#right_before_params_section
classes = ["aeroplane","bicycle","boat","cat","cow","diningtable","dog","horse","motorbike","sofa"]
if(classChoice!=None):
    classesOC = classChoice
    
else:
    print("No selection of data provided. Used a selectionen.")
    classesOC = [3,2,1]
    #classChoice = 3

#paramssection
if((len(classesOC)>1) and (len(classesOC)<10)):
    classString = "multiple_classes"
elif(len(classesOC)==10):
    classString = "all_classes"
else: 
    classString = classes[classesOC[0]]




Additionally added balanced sampling: 
if(OVERFIT): #CREATES TRAIN AND VALIDATION-SPLIT 
    
    #new mode for getting representative subsample: 
    sample_perc = [x/sum(nsamples) for x in nsamples] #get percentage-distribution 
    if(len(nsamples)>1):
        classwise_nsamples = [math.ceil(x*NUM_IN_OVERFIT) for x in sample_perc] #get number of samples per class
        NUM_IN_OVERFIT = sum(classwise_nsamples) #overwrite NUM_IN_OVERFIT, as you use math.ceil
    else: 
        classwise_nsamples = [NUM_IN_OVERFIT]
        
    h = [torch.Generator() for i in range(len(classesOC))]
    for i in range(len(classesOC)):
        h[i].manual_seed(i)
        
    ofIDX = torch.zeros(0).to(torch.int32)
    valIDX = torch.zeros(0).to(torch.int32)
    offset = 0
    for num,instance in enumerate(nsamples):
        idx = torch.randperm(int(nsamples[num]-1),generator=h[num])
        t_idx = idx + offset #add offset for later indexing
        idx = t_idx[:classwise_nsamples[num]]
        vidx = t_idx[classwise_nsamples[num]:]
        ofIDX = torch.cat((ofIDX,idx),0)
        valIDX = torch.cat((valIDX,vidx),0)
        offset += instance
    
    #IDX = torch.randperm(len(train),generator=g[0])#[:NUM_IN_OVERFIT].unsqueeze(1) #random permutation, followed by sampling and unsqueezing
    #ofIDX = IDX[:NUM_IN_OVERFIT].unsqueeze(1)
    #valIDX = IDX[NUM_IN_OVERFIT:NUM_IN_OVERFIT+17].unsqueeze(1) #17 because the smallest train-set has length 33=max(L)+17.
    overfitSet = torch.utils.data.Subset(train,ofIDX)
    valSet = torch.utils.data.Subset(train,valIDX)
    trainloader = DataLoader(overfitSet,batch_size=BATCH_SZ,shuffle=True,num_workers=0,generator=g)
    print("Overwrote trainloader with overfit-set of length {}".format(ofIDX.shape[0]))
    valloader = DataLoader(valSet,batch_size=BATCH_SZ,shuffle=True,num_workers=0,generator=g)
    print("Valloader of constant length {}".format(valIDX.shape[0]))


Softmax eller sigmoid 
Overvej at ændre titel til learning to localize objects from eye tracking data

Kig på resultaterne. Er de fine nok? 
Hvis nej: 
- Samme type overfitting som med fly, men med få billeder fra alle billeder
- Single-batch
	- Hvis gode resultater; træn på det hele 
	- Hvis overfitting på få samples ikke lykkedes, ændrer arkitektur

- Skrevet en masse 
- Hvis det bare ikke virker; det kan være dataen er for variabel. Så prøv at træne rigtigt på hele flydata-set uden overfit med regularizering - for at se om klassevis-model er mulig.

Så til næste gang: 
- Træne EF færdig 
	- Afhængigt af resultat, træne flere modeller
	- Kun fly, eller på det hele 
- Skrive ViT-metodik (8 timer) 
- Skrive lidt mere fra toppen (Objectives ind, fortsat introduktion, begynde på teori ) (8 timer)
- Billede med predictions m. eyetracking-punkter for at illustrere hvad der går galt for GT dårlige 
- Kigge nok på data til at se; er vi et sted hvor det er godt nok, eller kan vi begynde på analyse og diskussion 
	- Introduktion skal indeholde alt, men kort (3 linjer og en enkelt ligning)



STATUS: 
center / c2b funktioner virker ikke. 
Eksperiment kører på git HPC_UNSTABLE 

