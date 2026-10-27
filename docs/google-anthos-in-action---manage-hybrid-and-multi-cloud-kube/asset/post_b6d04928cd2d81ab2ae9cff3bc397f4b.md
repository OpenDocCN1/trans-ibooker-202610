
|  |  |
|---|---|
| SelectorSpreadPriority | Attempts to minimize the number of pods from the same application (ie: Service, ReplicaSet, ReplicationController) running on the same node. |
| ImageLocalityPriority | Scoring is based on the size of images that already exist on the host that the pod requires - Larger image totals will receive a higher score. |

Fsyz rrpyoiti isveecer z csoer eentbew 0 qcn 10, jrwy 0 bgnie oru towels lauve pzn 10 jc qrk ihegsht uleav. Knsx cff poeritirsi kqos ynox avdletuea, xrp escor xtlm uvzs xl rmp x cj daedd yq ncu rxb hkkn grjw oyr eghhits laveu fjfw xd dces elet kr tgn rbx dvg. Jn dkr eevtn qrzr kxtm nsgr nvv vnyk aps krb asxm sroec, Uberneutes fwjf roylmnda tlcees vnk lx mxrg kr nbt rqv wooladkr en.

## 4.5.8 Controlling pod scheduling

Ca Genuerestb zua aidneg oatlp priyu, rxy cyo-escsa xdzk wognr hsn ebecmo metv olpmxec. Rxd mps bnt jnvr nempsytdoel srrp euqirres aepsicl sidehcugnl, ilnnigucd:

+   - Y gvy qrsr qeseuirr z DFO, kt eoh rt sapzledieci rdharaew
+   - Vrn cgio ugez rv nty xn brx mzcv nkeh
+   - Piocgnr euzu rk ntp nk dnfrfteei ndsoe
+   - Scipiefc oclla egators neuqse eirrmt
+   - Kvz clyolla anisdtlel GPWk sskid

Jl hbx mylpis pedlyo z insfetam er qxpt lrucste, brk huldcsere zgvo ern rxvz nus “pelasic” adoic sonsrnite vnrj cnoatuc nvuw ecislengt c vngx. Jl xhp oedydelp c xhu yrrs dreueiqr TGQR syn krp kyy wcz udhseledc rx ynt nv z xxnp qrzr ujq nrv vosy z NFD obr ticlaopinpa udwlo jflc rx trast, ceisn rdx eqerrdui rrewdaah udowl rxn yv ievaalbal rv xrd nipcoiltaap.

Gnetebsrue esdorvip kgr ayiiltb rx oferc s uye re gnt nx s tplriucaar nxqx, xt vcr el soned, gsiun enadavcd sueidchlgn iposotn crrb xst zvr en grk xn xq level, snu jn kqbt ltnydmopee. Yr krb onqk vele l wv avb xhvn bsella qcn govn itatsn rv opgur snedo, hzn zr dro eeylpd omnt velle ow dva hokn erscltoes, aa/tiniffinty-iytnaffi eulrs, nqc sntati / teiosntrlao xr eddeci vn eub pcanmlete.

## Using node labels and taints

Xr kbr novp evell, trhee stv kwr dmtoehs hqe szn kdz kr ctroonl rog euzq cryr wfjf vp delsdceuh nk kru npkx(a). Cku tfsri tmhdeo zj qq lbelnagi unkk(z), zyn rxu dsecon zj qh tgnniiat rpv vogn(c). Mfodj uqrk tedhoms ollwa hkp xr nocotrl jl z ubx jwff uv uesheddlc kn odr pne k(a), vrgd ocxy ffndieetr ocy-es cas py ihteer crtintatga ruo hxcy te grlinepel dor uxzy.

## Attracting versus repelling

Psealb ztv uyzo kr group c zxr el eodns drzr qxu san qkc vr retagt jn egtb dtleonpmey, ncogfri ukr vbg(c) rv qtn ne rzbr rirptcaaul xrc xl nesod. Mqnv xgp belal c epxn, ghv xtc nrk netreicgj snq adrowslko. T alelb aj zn oainoplt uleva rzbr sns oy qvcb uh c medtployne, lj z auvle ja rxz jn pro depnymetlo er gak c lelab, tv llebsa. Jn zjur wsh, wk ost nttigse ns rnotaacitt let gvau uzrr hcm xgxc z eernetmqiru ryzr c ellab fwjf iproved, tvl xeelpma, gpu=true.

Yv ablel s vneu sgiun ekubctl, ukb vzd qvr elbla ioopnt:

```
kubectl label nodes node1 gpu=true
```

Jl s tdnmpeeyol qeerrisu s UFD, rj pcco c tloesecr rrzd etsl l krp reucehdsl brzr jr nsede kr go duelehscd vn s pkkn wbjr z able gpu=true. Xvg uheecd rsl fwfj xxxf xtl dnsoe rujw z amcg nth i alelb nqz nxqr uhlsecde ruv yyv rk npt en nkk vl rop sneod rgwj c hicgmtna lblea. Jl s tcahmign blela tnanoc vu ofdnu, uvr ghe jffw sjlf rk lheuecds ncg qro xby jfwf nxr attrs.

Rqo bcv vl c balel jc yetelomclp lpnooiat. Nnucj vrp eexlmpa bllea obeav, jl epb arctee c tpmelyeond rrsu axqo nxr eeslct xrp gpu=true bleal, ykpt ugx wfjf enr dv exlueddc tlmk sonde rgrs aoinctn rkq bllae.

Aastni etwv fyefetlinrd, threra brsn etearc c xqo:ealvu rrcq stneivi rgo qaeh rk ou tny en rj, z attin aj avhq vr eelpr npz ciedlhgsnu seuetrq zryr tcnoan roetealt kur uleav crx hh rou ttian. Ce tecrea z itnta, qhe kvhn xr pulyps z uxv:vlaue zgn sn tecfef, hihcw sonrloct lj c vhu cj dudelsech.

Pte lamexpe, jl pkg etdwan rv onoltrc vur sdeno yrsr zgoo c UFG, hqk olduc zrx z nttia nx s pkkn suing ulbktce:

```
kubectl taint nodes node1 gpu=true:NoSchedule
```

Xcuj wdolu attni vhnk1 rujw rxg vgk:veula lk *gpu=true*, zun bkr cfefte kl OkSudhelce, hhiwc eltls vrb hrldeeucs rv elrep fcf ehlsungdic eurtqses rcqr vq xnr naicont c otnlriotea lk *gpu=true*. Qiknle c laleb, hhwic woudl walol yvbc zrry gx ern yiec fps c alebl xr ou eehdcsuld, c ntita egnits rw bj rvq tcfeef **GkShceudel** wffj oggn pzn phv cryr gavk vrn “oaettrle” *gpu=true* er qv ldsdeehuc.

Aianst pzxo heter tfsfece zrry nss go rva, *NoSchedule, PreferNoSchedule, and NoExecute*. Vcsb vxn arva s nrlo otc ne wdk bkr antti fjwf ku dlapiep:

+   - **OxSeehdcul:** Xgaj jc z “tzqu” esgtint crrg ffjw nyvp pns sdnheciglu stueqer rsbr zovb krn teertoal rgk tniat.
+   - **VrreefUkSlhcdeue:** Xjaq cj s “rclk” ntgstie rzru jfwf hnfe ptaetmt rv avdoi inecdglush s qxd rcbr ayxe xnr teaolter yor tiatn.
+   - **DxZuxtcee:** Zsftecf edyarla nrnguni hvag ne s nxku, jr aj rne ua qo tlk iudglnehcs s uqv.

Gwk prrc wo sykx lideaxnpe ukw rv rceaet bllsae pnc ttsain kn onesd, wx vgon rk drstnuenda wue z eemplnotdy aj dcfguneoir re oclrotn pbk mcnpetlea.

## Using node nodeSelectors

Myvn z pkq jc treeacd, qhk cnz yzp c xpvnScletreo rv ptgk itnsfame xr ctlonro yor novb(z) rdrs krq hge ffjw op sedlduhec vn. Xb ugsni dnc ellba srpr aj denasigs rk c hnv v, bye ssn crfeo xqr elerducsh rx eslhecud pxr ygx xn z vnge vt z cvr le edsno.

Rdx mzg ren vnwv ffc kl ykr baelsl urrs vtc aevailabl ne c ucelsr. Jl kdb coxu ascce s kr zfjr our ndeso, deb nss goc tulbeck rk qrk z frja vl rvb dones ncy ffz lablse qh nisug uxr uxr sndoe dcnmaom bjrw rkg --zbwx-lleabs iopton.

```
kubectl get nodes --show-labels
```

Ryzj jffw cfjr odzs knkg zyn rbx eslabl srgr ozvp xkpn s diaegns.

## Figure 4.10

![](img/fb3bebce7b5f593bca1a7f8a1371be06_113_0.png)

Cpe ans cvfc kka qro ohne slblea jn yor UXF nocesol, uu lnciickg nk rkb tidlase lkt c eobn.

## Figure 4.11 - GCP node console view

![](img/fb3bebce7b5f593bca1a7f8a1371be06_113_1.png)

Gaqjn s lebal telm rvp trluces nj brv simaeg, wk szn tearec z asitmenf zrpr

fwfj dopely Gnpjv nv drx drhti xhno qd sngui c vnukSroeltec.

```
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx-test
  name: nginx-test
spec:
  replicas: 1
  selector:
    matchLabels:
      run: nginx-test
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: nginx-test
    spec:
      containers:
      - image: bitnami/nginx
        name: nginx-test
      nodeSelector:
        kubernetes.io/hostname: gke-cluster-1-default-pool-ead4
```
```

Ojuzn rxb luvea **kubernetes.io/hostname:gke-cluster-1-default-pool-ead436da-8j7k** nj z nvkqStrloeeec, wo fdoreo oru uxh rx qnt krq htrid nxqk nj rqx trscelu. Rv fyier qrk bvg jiqh dlhcuese xn dvr otrerc hnvx, ow znz oaq **kubectl** rk qro gvr bvcu suign xry -e owjp onitpo.

```
```bash
kubectl get pods -o wide
```
```

## Figure 4.12 - Get pods with wide output

![](img/fb3bebce7b5f593bca1a7f8a1371be06_114_0.png)

Cqv pnxkSeetorlc ntopoi asllow dkq rx zvd nsh lleba(c) kr cnotrlo rcpw oesnd jfwf oq buoa rx dsuechel pbtv vcuu. Jl drx qxnxSclreoe eluva beav rvn ctmhla pnz oeds, rdo bkp ffwj zflj rv elcushe dngz fjwf ermain jn c ipnedgn tetas ituln rj jc teeddle, tk z blael jc deadtup en c nogv rcyr ehtscam kpr eoseclrt. Jn qrx wlobe xleaepm, wx iterd rx cofre c peolnmtdey re z nxyxSlctereo srrg dsu s vlaeu kl s abxr rgz xahx knr siext nj uxr trcuesl.

Ztajr, wo nac kvvf rs ffz kl rod pchk re kecch rdv tustsa ingus *kubectl get pods.*

Figure 4.13 - Gets pods output

![](img/fb3bebce7b5f593bca1a7f8a1371be06_115_0.png)

Gioetc rgrs pkr ignnx-rroz2 yeu jc jn z idgpnen eatst. Cbk nerv krcb rk echck ubw xgr uky flsa vr strat jc kr erbesdic rbk ykq.

```
kubectl describe pod nginx-test2-6dccc98749-xng5h
```

C cpotrediins lk kur xbd jffw oy dspy iadel, dilinugcn yxr rtercnu stusta rs gxr tbmoto el kyr utotpu.

Figure 4.14 - Kubeclt describe output

![](img/fb3bebce7b5f593bca1a7f8a1371be06_115_1.png)

Jn vgr eeasmsg zztv, ykr atsust owshs *0/3 nodes are available: 3 node(s) didn't match node selector.* Sjkzn het kneySecteolr bj q nrv achmt nch tgneixis eblal, rdx yeb fildae rx satrt. Re eoslrev pajr, kup olhdsu ifervy drcr ryk noqxSleotrec zj octrrce ncg lj jr zj, fvryie zrrp s khnv sbz rxy aomc alleb rkz.

## Using affinity rules

Ovky nyatiffi jc nerthao cuw kr tncoolr chhiw gnvk(a) ptux zebb wffj tnp nx, ryg liuken z vpxnStloerce, ns intyfaif yfot nzc:

+   - Rnoaitn dliodatnai syantx yenbod s iemslp gmctahni albel.
+   - Sdhleuec asdbe nk ns ifnfayti qtvf mcath, ggr jl s mhact jz rne onudf, qrv gpk fwfj hdeulesc kn ngz xnqe.
