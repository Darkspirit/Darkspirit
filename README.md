### Hi there 👋

Please support the following [memory safe](https://www.memorysafety.org/docs/memory-safety/) Rust projects. It would be great to have such a future:
* [Redox OS](https://github.com/redox-os)
  * [relibc](https://github.com/redox-os/relibc) C standard library written in Rust
  * [reqwest](https://github.com/seanmonstar/reqwest) instead of curl
  * [Servo](https://github.com/servo/servo) web browser
    * [WebRender](https://github.com/servo/webrender)
    * [Pathfinder](https://github.com/servo/pathfinder) for Canvas, Font, SVG
    * [Rustls](https://github.com/ctz/rustls)
      * [graviola](https://github.com/ctz/graviola) for x86_64 + arm64
      * [aws-lc-rs](https://github.com/aws/aws-lc-rs) or [ring](https://github.com/briansmith/ring) for the rest (riscv64, wasm32, loongarch64, powerpc64, s390x, mips, x86, arm32)
      * [Rustls-WebPKI](https://github.com/rustls/webpki) (based on [WebPKI](https://github.com/briansmith/webpki))
    * [rust-av](https://github.com/rust-av) (hopefully as [media backend](https://github.com/servo/media/tree/master/backends) sometime in the future)
      * [mkv/webm](https://github.com/rust-av/matroska), mp4
      * [Playback](https://github.com/rust-av/avp)
        * [Opus](https://github.com/lu-zero/opus), FLAC
        * [AV1](https://github.com/rust-av/dav1d-rs) (soon [rav1d](https://github.com/memorysafety/rav1d)), [VP8/VP9](https://github.com/rust-av/vpx-rs)
      * [Transcoding](https://github.com/rust-av/ave)
        * [rav1e](https://github.com/xiph/rav1e)
    * [boa](https://github.com/boa-dev/boa) (hopefully as safe JS interpreter in the far future)
  * [Hickory DNS](https://github.com/hickory-dns/hickory-dns) (former name was [Trust-DNS](https://github.com/bluejekyll/trust-dns))
  * [Stalwart server](https://github.com/stalwartlabs/mail-server): mail (JMAP,IMAP,SMTP,DKIM,DMARC,etc), contacts/calendars/files (WebDAV, later also JMAP), ACME client
  * [warp](https://github.com/seanmonstar/warp) or [axum](https://github.com/tokio-rs/axum) web server framework
    * [Yew](https://github.com/yewstack/yew) or [Leptos](https://github.com/leptos-rs/leptos) web app framework
* Please be green on [hardenize.com](https://hardenize.com) and use IPv6 (against IPv4 fragmentation attacks), [RPKI](https://nlnetlabs.nl/projects/rpki/routinator/) for BGP, [ECDSA DNSSEC](https://www.cloudflare.com/dns/dnssec/ecdsa-and-dnssec/) ([black lies](https://blog.cloudflare.com/black-lies/) instead of classic NSEC/NSEC3), TLSA/DANE, TLS 1.3, DMARC (`p=reject`), SPF (`mx -all`), [CSP](https://report-uri.com/home/generate), [XHTML5](https://blog.whatwg.org/xhtml5-in-a-nutshell),
  * CAA
    * `CAA 128 issue "letsencrypt.org; validationmethods=dns-01"`
    * `CAA 0 issuemail ";"` [only issuemail can restrict S/MIME cert issuance](https://www.rfc-editor.org/rfc/rfc9495.html#name-no-issuemail-property)
    * `CAA 0 issuevmc ";"` [only issuevmc can restrict issuance of a signed maildomain logo](https://bimigroup.org/resources/VMC_Requirements_latest.pdf#page=59) for the [DNS TXT BIMI record](https://support.google.com/a/answer/10911321)
  * HSTS
    * HTTP header+[preload](https://hstspreload.org/): `strict-transport-security: max-age=31536000; includesubdomains; preload`[[1]](https://datatracker.ietf.org/doc/html/rfc7540#:~:text=However%2C%20header%20field%20names%20MUST%20be%20converted%20to%20lowercase%20prior%20to%20their%20encoding%20in%20HTTP%2F2%2E)[[2]](https://datatracker.ietf.org/doc/html/rfc9114#:~:text=Characters%20in%20field%20names%20MUST%20be%20converted%20to%20lowercase%20prior%20to%20their%20encoding%2E)[[3]](https://datatracker.ietf.org/doc/html/rfc6797#:~:text=Directive%20names%20are%20case%2Dinsensitive%2E)[[4]](https://www.rfc-editor.org/rfc/rfc9204.html#:~:text=max%2Dage%3D31536000%3B%20includesubdomains%3B%20preload) 
    * HSTS via HTTPS DNS RR: `HTTPS 1 . alpn=h2 no-default-alpn` (h2) or `HTTPS 1 . alpn=h2` (h2,h1)
      * or with h3 `HTTPS 1 . alpn=h3,h2 no-default-alpn` (h3,h2) or `HTTPS 1 . alpn=h3,h2` (h3,h2,h1)
      * and in the future `HTTPS 1 . alpn=h3 no-default-alpn` (h3)
  * Against UDP (QUIC) reflection/amplification attacks: [Require](https://github.com/quinn-rs/quinn/blob/fe596df2c4402afe2c72a7bb5628ec0fc6298021/quinn/examples/server.rs#L153-L155) a QUIC [Retry token](https://datatracker.ietf.org/doc/html/rfc9000#:~:text=A%20server%20MAY%20send%20Retry%20packets%20in%20response%20to%20Initial%20and%200%2DRTT%20packets%2E) (it's like a TCP SYN Cookie, otherwise the server would spam its TLS certificate to unvalidated IP addresses). For HTTP3 [here](https://github.com/hyperium/h3/blob/0e54a16df76102809ad99b03663b559dfc203917/examples/server.rs#L107).
    * [HTTP3 can better handle packet loss + client IP change](https://calendar.perfplanet.com/2020/head-of-line-blocking-in-quic-and-http-3-the-details/): HTTP1.1 head-of-line blocking is solved by HTTP2 multiplexing, but it still suffers from TCP head-of-line blocking in case of packet loss. HTTP3 solves that by using UDP QUIC.
    * [HTTP3/QUIC pro and contra](https://calendar.perfplanet.com/2018/quic-and-http-3-too-big-to-fail/)
* Hardware
  * Against [Side-channel attacks](https://en.wikipedia.org/wiki/Side-channel_attack): Use CPUs that don't have [Simultaneous multithreading](https://en.wikipedia.org/wiki/Simultaneous_multithreading) (SMT = more than 1 thread per physical core / multiple virtual cores per physical core / [Intel Hyper-Threading](https://en.wikipedia.org/wiki/Hyper-threading))

---

* Organic hardware health security (in progress)
  * Fat (maximum [30 to 35%](https://www.dge.de/gesunde-ernaehrung/faq/fettleitline/#c3311) of daily kcal consumption, e.g. 0,35*1633=571.55 kcal)
    * Fat -> liver -> ketone bodies as energy
    * If too little kcal/day: body fat becomes energy if insulin level is low
      * (=in case you eat a bit carbohydrates or sugar, way less fat becomes energy, even more muscle mass is consumed and hunger is higher)
    * If too much kcal/day (from sugar, carbohydrates, protein or fat): fat becomes body fat if you eat more kcal than needed
      * resting metabolic rate: idle condition = 1 kcal per hour per kg body weight = e.g. 65kg * 24kcal = 1560kcal
      * basal metabolic rate = Harris–Benedict equation:
        * men: 66,5 + (13,75 * weight in kg) + (5,003 * height in cm) - (6,75 * age)
          * e.g. 66,5 + (13,75 * 65 kg) + (5,003 * 175 cm) - (6,75 * 30 years) = 1633 kcal per day
        * women: 655,1 + (9,563 * weight in kg) + (1,850 * height in cm) - (4,676 * age)
          * e.g. 655,1 + (9,563 * 65 kg) + (1,850 * 175 cm) - (4,676 * 30 years) = 1460 kcal per day
    * saturated fatty acids (maximum [7 to 10%](https://www.dge.de/gesunde-ernaehrung/faq/fettleitline/#c3312) of daily kcal consumption, e.g. 0,07*1633=114.31 kcal)
      * pro-inflammatory
        * inflammation causes cancer
      * rather solid at room temperature
      * land animal fat
      * 80-90% of coconut oil (vegetarian butter, cheese)
    * unsaturated fatty acids
      * monounsaturated fatty acids
        * Omega-9
          * anti-inflammatory
          * oleic acid
            * olives / olive oil (rather for cold cooking. safe with distance below smoke point of [130°C-190°C](https://de.wikipedia.org/wiki/Rauchpunkt), you might reach it and harmful substances form.)
      * polyunsaturated fatty acids (maximum [10%](https://www.dge.de/gesunde-ernaehrung/faq/fettleitline/#c3312) of daily kcal consumption, e.g. 0,1*1633=163.3 kcal)
        * Omega-6
          * If too much: pro-inflammatory (but too less Omega-6 would be unhealthy as well)
            * 50:1 (even 10:1) Omega-6 to Omega-3 ratio is pro-inflammatory and frequent in the West
            * recommended maximum is 5:1, ideal would be 1:1.
            * The same enzyme processes Omega 6 and 3: If all enzymes are "occupied" with omega-6, the body cannot absorb omega-3.
          * sunflower oil (safe with distance below smoke point of [232-252°C](https://de.wikipedia.org/wiki/Rauchpunkt))
          * Colza/Rape oil for frying (also contains some Omega-3 ALA. safe with distance below smoke point of [190-230°C](https://de.wikipedia.org/wiki/Rauchpunkt))
        * Omega-3
          * [anti-inflammatory](https://www.annualreviews.org/content/journals/10.1146/annurev-food-111317-095850)
          * ALA
            * Linseed/Flaxseed oil for cold cooking (safe with distance below smoke point of [~107°C](https://de.wikipedia.org/wiki/Rauchpunkt): Must not be used for frying, otherwise harmful substances form.)
            * Colza/Rape oil for frying (contains more Omega-6 than Omega-3 ALA. safe with distance below smoke point of [190-230°C](https://de.wikipedia.org/wiki/Rauchpunkt))
          * EPA+DHA from Schizochytrium microalgae [[1]](https://www.rossmann.de/de/gesundheit-doppelherz-omega-3-1000-vegan-kapseln/p/4009932132786) [[2]](https://www.rossmann.de/de/gesundheit-altapharma-omega-3-algenoel/p/4305615808970)
            * for brain, eyes, heart, skin
            * 0,25–0,5g/day recommended. up to 5g/day definitely harmless according to [EFSA](https://www.efsa.europa.eu/de/press/news/120727) who did not define an upper limit
            * alternatives:
              | fish | g EPA/<br/>100g | g DHA/<br/>100g | g ALA/<br/>100g | g Ome<br/>ga-6/<br/>100g | 6/3 ratio |  |
              |------|----------|----------|----------|--------------|---|---|
              | Coastal<br/>sardines | 0,5–1,0 | 0,7–1,2 | 0,1–0,3 | 0,1–0,3 | 0,1:1 ✅ | High Omega-3, thin bones (often eaten) |
              | Coastal<br/>sprats | 0,6–1,1 | 0,8–1,3 | 0,1–0,3 | 0,1–0,3 | 0,1:1 ✅ | High Omega-3, thin + belly bones (often eaten) |
              | Coastal+ offshore<br/>Atlantic herring | 0,7–1,0 | 0,9–1,2 | 0,2–0,4 | 0,2–0,4 | 0,1:1 ✅ | High Omega-3, bigger bones (need preparation) |
              | Coastal<br/>mackerel | 0,6–1,2 | 0,9–1,5 | 0,1–0,3 | 0,2–0,5 | 0,2:1 ✅ | High Omega-3, bigger bones (need preparation), sustainable if certified |
              | Saltwater<br/>net cage aquaculture<br/>salmon | 0,4–0,8 | 0,8–1,4 | 0,1–0,2 | 0,3–0,6 | 0,2:1 ✅ | High Omega-3 (needs fish flour from sardines/sprats/etc as food for high Omega-3), mild taste, medium bones (easily removed), sustainable if certified, possible contaminants |
              | Freshwater aquaculture<br/>eel | 0,2–0,4 | 0,3–0,6 | 0,1–0,2 | 0,4–0,8 | 0,7:1 ⚠️ | Moderate Omega-3, high fat content, possible contaminants |
              | Alaska pollack<br/>(fish sticks) | 0,1–0,2 | 0,2–0,3 | 0,05–0,1 | 0,05–0,1 | 0,2:1 ✅ | Moderate Omega-3, mild taste, small bones (easily removed), sustainable if certified |
              | Freshwater<br/>trout | 0,1–0,3 | 0,3–0,7 | 0,1–0,2 | 0,2–0,4 | 0,4:1 ☑️ | Moderate Omega-3, milder taste |
              | Freshwater<br/>chub | 0,1–0,2 | 0,2–0,3 | 0,05–0,1 | 0,05–0,1 | 0,2:1 ✅ | Lower Omega-3, less common |
            * bad alternatives:
              * large fish (high toxic methylmercury): Tuna (0,4–0,8g DHA/100g, 0,1–0,3g EPA/100g, 0,05–0,1g ALA, 0,05–0,1g Omega-6), Wild salmon
              * critically endangered + more toxic methylmercury: wild(!) freshwater eel (moderate Omega-3, high fat content)
  * Protein
    * Amino acids
    * If too much -> becomes sugar -> insulin goes up -> sugar becomes fat
    * If too little -> muscles are consumed
  * Carbohydrates
    * Indigestible carbohydrates (called fiber)
    * Digestible carbohydrates (the actual Carbohydrates)
      * Carbohydrates become sugar -> insulin goes up -> sugar becomes fat
  * Sugar
    * Sugar -> insulin goes up -> sugar becomes fat
    * ⚠️⚠️ Sugar + lactic acid bacteria that are anyway present in the mouth = lactic acid + more bacteria = pH at/below 5.5 = tooth enamel demineralizes (teeth become slightly yellow because the shell irreversibly becomes thinner, opaque white spot on tooth surface) + caries risk (yellow/brown/black)
  * Minerals
    * Salt
      * rises blood pressure: vascular damage. risk of heart attacks and other cardiovascular diseases
  * Vitamins and Minerals
    * [[link]](https://www.rossmann.de/de/gesundheit-altapharma-a-z-depot-ab-50/p/4305615887050)
      * Vitamins
        * Vitamin A
        * Vitamin B1 (Thiamin)
        * Vitamin B2 (Riboflavin)
        * Vitamin B3 (Niacin)
        * Vitamin B5 (Pantothenic acid)
        * Vitamin B6 (Pyridoxine)
        * Vitamin B7 (Biotin)
        * Vitamin B9=B11 (Folic acid)
        * Vitamin B12 (Cobalamin)
        * Vitamin C (Ascorbic acid)
        * Vitamin D3 (Cholecalciferol)
        * Vitamin E
        * Vitamin K1 (Phytomenadione)
      * Minerals
        * Calcium
        * Phosphorous
        * Magnesium for muscles, brain+nerve system, liver, kidneys, blood pressure, heart, bones+teeth
        * Zinc for eyes, brain, skin, bones, hair, muscles
      * Trace elements
        * Chromium
        * Iodine
        * Molybdenum
        * Selenium
      * Lutein for eyes
    * [[link]](https://www.rossmann.de/de/gesundheit-zirkulin-leber-vital-hochdosiert/p/4036581253388)
      * Silymarin (for liver: anti-inflammatory antioxidant)
      * Vitamin B4 (Choline) for liver, brain+nerve system
      * Zinc
  * Physical activity: [Any movement](https://www.youtube.com/watch?v=Wmo7M9I4fGc&t=6327s) that gets the circulation going for at least 10 minutes (makes the heart beat a little faster) temporarily brings the immune system's T cells out of their resting place (lymph nodes and bone marrow) into the bloodstream and lets them go on patrol which helps against cancer.
  * ⚠️⚠️ At and below a pH value of 5.5, tooth enamel is irreversibly damaged, it begins to dissolve/demineralize (teeth become slightly yellow because the shell irreversibly becomes thinner, opaque white spot on tooth surface).
    * [later it looks like abraded, you only have the yellow core of the tooth left](https://medisiegel.de/gesundheit/zahnerosion)
    * caries risk (yellow/brown/black)
    * Don't have it multiple times a day, ideally not even daily.
    * Cover your teeth with saliva immediately after drinking/eating
      * Saliva contains calcium and phosphate ions which, together with fluoride toothpaste, lead to tooth enamel remineralization (it prevents further yellowization, nothing more).
        * Bleaching would be required for getting them white again, but the damage can't be repaired, the shell can't be made thicker again.
    * ⚠️⚠️ Phosphoric acid: Cola, Lemonades, iced teas
    * ⚠️⚠️ Citric acid: energy drinks (btw: mouthwash+energydrink=sulfur compounds that smell like rotten egg), sour gummy bears, candies and lollipops, citrus fruits
    * Malic acid: apples
    * ⚠️⚠️Lactic acid: When lactic acid bacteria that are anyway present in the mouth in small numbers come into contact with sugar they reproduce and produce acids (mainly lactic acid).
  * ⚠️⚠️⚠️ Alcohol
    * There is [no risk-free](https://www.dge.de/presse/meldungen/2024/dge-positionspapier-zu-alkohol/) amount of consumption: Recommended is zero.
    * Alcohol breakdown products from your liver destroy your DNA: mutation leads to cancer.
    * Alcohol disinfects because it lets cells explode. Alcohol destroys upper layers in your mouth, lower layers need to grow to replace them.
      * When you smoke (smoke contains more than 60 carcinogens that cause cancer) and consume alcohol, then your cancer risk is [100 times higher](https://youtu.be/Wmo7M9I4fGc?feature=shared&t=4904) compared to only smoking.
    * [~13%](https://youtu.be/Wmo7M9I4fGc?feature=shared&t=4990) of breast cancer cases are caused by alcohol
    * Pregnant women who drink alcohol [only for 1 day](https://www.youtube.com/watch?v=Wmo7M9I4fGc&t=5230s) can already make their child violent and disabled.
  * Clean Air
    * ⚠️⚠️⚠️ Microplastics and fine dust from car tire wear, non-magnetic train tire wear, combustion exhaust gases (wood stoves, combustion cars, gas and oil heating, coal-fired power plants) and laundry fibers can cause:
      * neurological damage (microplastics and fine dust get into your blood and even into your brain, they can pass the blood-brain barrier)
      * intestinal/lung damage, asthma, silicosis, Pneumonia, COPD, lung cancer
      * hormonal disorders
      * heart disease, heart attacks, strokes, cancer
      * infant mortality
      * Alternatives
        * R744 (CO2) or R290 (Propane) Heat Pump for heating and cooling with hot+cold water(R718) distribution pipes and [fan coils](https://www.vaillant.at/privatanwender/produkte/hydraulischer-geblase-konvektor-fancoil-arovair-146880.html) (difference to air-air aircon is only a very few percents/minutes) that ideally have a HEPA air filter (have only found some with less good filters so far)
          * [Public R744 Large Heat Pumps can produce heat+cold, store heat, reconvert heat into electrity](https://www.youtube.com/watch?v=HvuGS0c7sIs&t=418s)
          * R290 heat pump (e.g. [air-water](https://www.vaillant.de/heizung/produkte/luft-wasser-waermepumpe-arotherm-plus-397248.html), ground-water[[1]](https://www.stiebel-eltron.de/de/home/produkte-loesungen/erneuerbare_energien/waermepumpe/sole-wasser-waermepumpen/wpe-i-07-1-17-1-plus-h--230-/wpe-i-07-1-plus-h-230.html)[[2]](https://www.heliotherm.com/de/produkte/direktverdampfer-waermepumpe-natural-technology.html))
          * Natural refrigerants (R744, R290) don't cause reprotoxic PFAS ([TFA](https://www.umweltbundesamt.de/presse/pressemitteilungen/trifluoressigsaeure-tfa-bewertung-fuer-einstufung) is result of leakage of HFOs like R1234yf from car aircon), don't cause toxic hydrogen fluoride (result of candle fire with leakage of R32 from air-to-air aircon) and don't harm the climate (high-GWP (Global Warming Potential) HFC like R134a)
        * [magnetic train](https://magnetbahn.de/vorteile/), electric car
        * green energy
          * [AgriPV](https://www.ise.fraunhofer.de/de/geschaeftsfelder/solarkraftwerke-und-integrierte-photovoltaik/integrierte-photovoltaik/agri-photovoltaik-agri-pv.html) benefits: evaporation protection + hail protection + reduced pesticide need
            * Protein plants(peas etc: tasty Schnitzel)+catch crops
            * Using current biofuel land for agri-PV could provide electric cars [11600% (116x)](https://www.ise.fraunhofer.de/content/dam/ise/de/documents/publications/studies/aktuelle-fakten-zur-photovoltaik-in-deutschland.pdf#page=39) the driving range of biofuel.
          * BiodiversityPV: pollinators,insects,birds [[1]](https://blog.rheinenergie.com/blog/weltbienentag-am-20-mai-es-blueht-und-summt-unter-den-solaranlagen/2258) [[2]](https://www.bundeswirtschaftsministerium.de/Redaktion/DE/Downloads/J-L/leitfaden-naturschutzfachliche-mindestkriterien-bei-pv-freiflaechenanlagen.html)
            * Using current biofuel land for BiodiversityPV could provide electric cars 18900% (189x) the driving range of biofuel. (["Range is 190 times higher"](https://www.ise.fraunhofer.de/content/dam/ise/de/documents/publications/studies/aktuelle-fakten-zur-photovoltaik-in-deutschland.pdf#page=39))
          * Winter Wind, Winter vertical PV
          * public/private accus
          * Vehicle-to-Grid
          * Dark Doldrum Biomass(poplar instead of corn[[1]](https://group.vattenfall.com/de/newsroom/news/2022/jan-grundmann-forschungsprojekt-biomethan--torfersatzstoff-aus-pappelholz) [[2]](https://biogas.fnr.de/service/presse/presse/aktuelle-nachricht/zwei-fliegen-mit-einer-klappe-pappelholz-fuer-biomethan-und-torfersatzstoffe), sewage sludge/gas) [Biomethane](https://reverion.com/biogas-kraftwerke/) pyrolysis electricity (graphite -> graphene)
  * Vaccination:
    * HPV [vaccination](https://www.aok.de/gp/verordnung/wirtschaftlichkeit/impfungen-niedersachsen/hpv) makes sense: HPV (women+men) infects [all mucosae](https://www.youtube.com/watch?v=Wmo7M9I4fGc&t=5828s) which can cause cancer.

<!--
**Darkspirit/Darkspirit** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
