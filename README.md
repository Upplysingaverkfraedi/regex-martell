Verkefnið var unnið í VS Code. Þar var unnið bæði i markdown skjölum ásamt python. 
Spurning 1 er kóðuð í skjali  regex_kt.py. Til að keyra þann kóða þarf að framkvæma fara inn í code möppuna þannig "cd code" svo þarftu að keyra: 
python regex_kt.py --file inntaksskra.txt --einstaklingar eða 
python regex_kt.py --file inntaksskra.txt --fyrirtaeki 
þ.e. ef þú vilt leita af einstaklings kennitölu eða fyrirtækja. Til að finna báðar í einu geriru python regex_kt.py --file inntaksskra.txt --einstaklingar --fyrirtaeki 
Fræðilegi parturinn er unnin í skjali: lidur1ADALSKJALskilatessu.md þar sem segðin er hönnuð og útskýrð ítarlega. Textaskjalið inntaksskra.txt eða kt.txt (sá eftir á) inniheldur eftirfarandi upplýsingar: 
Kennitala Haskola Islands: 600169-2039
Vigdis Finnbogadottur: 150430-2329 
Guðni Th. Johannessonar: 260668-4719. 
Halla Tomasdóttir: 111068-4379
Kennitala Auður Capital: 640507-0390. 
Ekki logleg kennitala: 151617-1819.


Spurning 2 í skjali regex_email.py.
Mögulega þarf að nota þennan koda til að runa file fyrir sp2

python3 sp2.py --file data/email.txt


Spurning 3 í skjali regex_reorder.py
Spurning 3 sp3.py er python skjal fyrir dæmi 3. Keyra skrá með:

python3 code/regex_reorder.py --infile data/nafn_heimilisfang_simanumer.csv --outfile data/heimilisfang_simanumer_nafn.tsv

tekur inn t.d.

Jón Jónsson, Litla-Saurbæ, 816 Ölfusi, 555-1234 Guðrún Helgadóttir, Fiskislóð 15, 101 Reykjavík, 510-7000 Jón Oddur Guðmundsson, Úthlíð 6, 450 Patreksfirði, 897-1234

og breytir í

Litla-Saurbæ 816 Ölfusi 555-1234 Jónsson, Jón Fiskislóð 15 101 Reykjavík 510-7000 Helgadóttir, Guðrún Úthlíð 6 450 Patreksfirði 897-1234 Guðmundsson, Jón Oddur

Þá býr hann til skjal í data möppunni þar sem búið er að endur raða. skipunin tekur inn skránna nafn_heimilisfang_simanumer.csv og skilar skránni heimilisfang_simanumer_nafn.tsv (í data möppu).


Spurning 4 í timataka.py.
Notum:

python3 code/timataka.py --url "https://timataka.net/snaefellsjokulshlaupid2014/urslit/?race=1&cat=overall" --output data/hlaup.csv --debug

Til að keyra. Getur valið vefslóð af timataka.net

Getum notað

https://timataka.net/hyrox-06-2024/urslit/?race=1&cat=m&age_from=10&age_to=99&division=Kk%20pro

https://timataka.net/jokulsarhlaup2024/urslit/?race=2&cat=m

https://www.timataka.net/snaefellsjokulshlaupid2014/urslit/?race=1&cat=m&age=0039

Regluleg segð fyrir URL ^https://(www.)?timataka.net/[\w\d-]+/urslit/?race=\d+(&cat=\w+)(&age=\d+)?(&age_from=\d+&age_to=\d+)?(&division=\w+%20\w+)?$

Tekur inn vefslóð og skilar .csv skrá í hlaup.csv. Einnig er hægt að sjá hlaup.html sem er unnið úr.

