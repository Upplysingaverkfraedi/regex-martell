Spurning 3
sp3.py er python skjal fyrir dæmi 3.
Keyra skrá með:

python3 code/regex_reorder.py --infile data/nafn_heimilisfang_simanumer.csv --outfile data/heimilisfang_simanumer_nafn.tsv

tekur inn t.d.

Jón Jónsson, Litla-Saurbæ, 816 Ölfusi, 555-1234
Guðrún Helgadóttir, Fiskislóð 15, 101 Reykjavík, 510-7000
Jón Oddur Guðmundsson, Úthlíð 6, 450 Patreksfirði, 897-1234

og breytir í

Litla-Saurbæ 816 Ölfusi 555-1234 Jónsson, Jón
Fiskislóð 15 101 Reykjavík 510-7000 Helgadóttir, Guðrún
Úthlíð 6 450 Patreksfirði 897-1234 Guðmundsson, Jón Oddur

Þá býr hann til skjal í data möppunni þar sem búið er að endur raða. skipunin tekur inn skránna nafn_heimilisfang_simanumer.csv og skilar skránni heimilisfang_simanumer_nafn.tsv (í data möppu).

Spurning 4
Í skjali sp4.py
Við notum requests til að sækja HTML-skjalið af tímataka.net og reglulega segð til að greina gögnin. Lausnin greinir sjálfkrafa hvort keppnin sé einstaklingskeppni, liðakeppni eða hraðaniðurstöður og sækir viðeigandi gögn. Niðurstöðurnar eru svo vistaðar í CSV-skrá með pandas fyrir frekari greiningu. Auk þess er möguleiki á að vista HTML-skjalið til að skoða það betur. Lausnin tryggir að gögnin séu unnin rétt óháð keppnisformi.

Notum:

python3 code/timataka.py --url "https://timataka.net/snaefellsjokulshlaupid2014/urslit/?race=1&cat=overall" --output data/hlaup.csv --debug

Til að keyra. Getur valið vefslóð af timataka.net

Getum notað

https://timataka.net/hyrox-06-2024/urslit/?race=1&cat=m&age_from=10&age_to=99&division=Kk%20pro

https://timataka.net/jokulsarhlaup2024/urslit/?race=2&cat=m

https://www.timataka.net/snaefellsjokulshlaupid2014/urslit/?race=1&cat=m&age=0039

Regluleg segð fyrir URL
^https://(www.)?timataka.net/[\w\d-]+/urslit/?race=\d+(&cat=\w+)(&age=\d+)?(&age_from=\d+&age_to=\d+)?(&division=\w+%20\w+)?$

Tekur inn vefslóð og skilar .csv skrá í hlaup.csv. Einnig er hægt að sjá hlaup.html sem er unnið úr.

1. parse_individual_results function

Reglulegar segðir:

names: re.findall(r'<td[^>]*class="name"[^>]*>(.*?)<\/td>', html)
Leitar að öllum <td> reitum þar sem class="name", og dregur út innihald þeirra (nafnið á keppandanum).

years: re.findall(r'<td[^>]*class="year"[^>]*>(.*?)<\/td>', html)
Leitar að reitum með class="year", dregur út árið.

clubs: re.findall(r'<td[^>]*class="club"[^>]*>(.*?)<\/td>', html)
Leitar að reitum með class="club", dregur út klúbbinn.

times: re.findall(r'<td[^>]*class="time"[^>]*>(.*?)<\/td>', html)
Leitar að reitum með class="time", dregur út tímatöku keppandans.

behind: re.findall(r'<td[^>]*class="behind"[^>]*>(.*?)<\/td>', html)
Leitar að reitum með class="behind", dregur út hversu langt keppandinn er á eftir efsta manni.

Í þessari aðferð eru reglulegar segðir notaðar til að finna og sækja gögn úr hverjum reit sem tilheyrir tilteknum þáttum eins og nafni, ári, klúbbi o.s.frv. Gögnin eru síðan sett saman í lista af orðabókum þar sem hvert stak inniheldur upplýsingar um einn keppanda.

4. parse_team_results function

Reglulegar segðir:

teams: re.findall(r'<td[^>]*class="team"[^>]*>(.*?)<\/td>', html)
Leitar að liðunum með class="team".

members: re.findall(r'<td[^>]*class="members"[^>]*>(.*?)<\/td>', html, re.DOTALL)
Leitar að meðlimum liðsins með class="members". Notkun re.DOTALL tryggir að regluleg segð geti lesið fleiri línur (þar sem meðlimir geta verið á mörgum línum).

splits: re.findall(r'<td[^>]*class="split"[^>]*>(.*?)<\/td>', html, re.DOTALL)
Leitar að "splits", sem gæti verið millitímar eða svipaðar upplýsingar.

times: re.findall(r'<td[^>]*class="time"[^>]*>(.*?)<\/td>', html)
Leitar að tímatöku liðsins.

Hér er ferlið svipað og í einstaklingskeppninni nema að nú er verið að sækja gögn um lið, meðlimi þeirra, og millitíma.

6. parse_pace_results function
   
Reglulegar segðir:

names: re.findall(r'<td[^>]*class="name"[^>]*>(.*?)<\/td>', html)
Leitar að keppandanöfnum.

ages: re.findall(r'<td[^>]*class="age"[^>]*>(.*?)<\/td>', html)
Leitar að aldri keppenda með class="age".

final_times: re.findall(r'<td[^>]*class="final"[^>]*>(.*?)<\/td>', html)
Leitar að lokatímum keppenda.

paces: re.findall(r'<td[^>]*class="pace"[^>]*>(.*?)<\/td>', html)
Leitar að hraða keppenda.

Þessi aðferð notar reglulegar segðir til að finna hraða og aldur keppenda ásamt lokatímum þeirra.

8. parse_html function
   
Þessi aðferð ákvarðar hvernig á að vinna úr HTML gögnunum byggt á því hvaða tegund niðurstaðna er til staðar. Hún kallar síðan á rétta parsinga-aðferð (parse_individual_results, parse_team_results eða parse_pace_results).

Áhersla á reglulegar segðir:

<td[^>]*class="something"[^>]*>(.*?)<\/td>: 

Þessi mynstrin sækja allar <td> töflu frumur þar sem ákveðin class gildi eru til staðar, til dæmis class="name", class="year" o.s.frv.

[^>]*: 

Þetta hluti segir segðinni að leyfa hvaða aukaeiginleika sem er innan <td> merkisins.

(.*?):

Þetta táknar lágmarksgreedy (non-greedy) leitarviðfang til að fá innihaldið milli byrjun- og endamerkja frumunnar, þ.e. milli <td> og </td>.

Þannig eru reglulegar segðir notaðar til að einfalda greiningu HTML gagna, og leita að upplýsingum innan ákveðinna reita.
