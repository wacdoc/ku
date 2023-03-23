# Pêşkêşkara xweya şandina nameya SMTP-ê ava bikin

## pêşgotin

SMTP dikare rasterast karûbaran ji firoşkarên ewr bikire, wek:

* [Amazon SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali ewr push email](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Her weha hûn dikarin servera nameya xweya xwe ava bikin - şandina bêsînor, lêçûna giştî ya kêm.

Li jêr, em gav bi gav destnîşan dikin ka meriv çawa servera xweya nameyê ava dike.

## Hilbijartina server

Pêşkêşkara SMTP-ya xwe-mêvandar IP-ya gelemperî bi portên 25, 456, û 587 vekirî hewce dike.

Ewrên gelemperî yên ku bi gelemperî têne bikar anîn van portan ji hêla xwerû ve bloke kirine, û dibe ku meriv wan bi derxistina fermanek kar veke, lê ji ber vê yekê ew pir tengahî ye.

Ez pêşniyar dikim ku ji mêvandarek ku van benderan vekirî ye bikirin û piştgirî dide sazkirina navên domaina berevajî.

Li vir, ez [Contabo](https://contabo.com) pêşniyar dikim.

Contabo pêşkêşvanek mêvandariyê ye ku li Munich, Almanya ye, ku di sala 2003-an de bi bihayên pir pêşbazî hatî damezrandin.

Ger hûn Euro wekî pereyê kirînê hilbijêrin, dê bihayê erzantir be (serverek bi bîranîna 8 GB û 4 CPU salane bi qasî 529 yuan lêçûn e, û heqê sazkirinê ya destpêkê salek belaş e).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

Dema ku fermanê didin, têbînî `prefer AMD` , û servera bi AMD CPU dê performansa çêtir hebe.

Li jêr, ez ê VPS-a Contabo-yê wekî mînakek bigirim da ku destnîşan bikim ka meriv çawa servera xweya e-nameya xwe ava dike.

## Veavakirina pergala Ubuntu

Pergala xebitandinê li vir Ubuntu 22.04 e

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

Ger server li ser ssh nîşan bide `Welcome to TinyCore 13!` (wek ku di wêneya jêrîn de tê xuyang kirin), ev tê vê wateyê ku pergal hîn nehatiye saz kirin. Ji kerema xwe ssh-ê qut bikin û çend hûrdeman li bendê bimînin ku hûn dîsa têkevinê.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

Dema ku `Welcome to Ubuntu 22.04.1 LTS` xuya dibe, destpêkkirin qediya, û hûn dikarin bi gavên jêrîn bidomînin.

### [Bijarte] Jîngeha pêşkeftinê bidin destpêkirin

Ev gav vebijarkî ye.

Ji bo rehetiyê, min sazkirin û veavakirina pergalê ya nermalava ubuntuyê danî [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Fermana jêrîn bimeşînin da ku bi yek klîk saz bikin.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Bikarhênerên Çînî, ji kerema xwe li şûna wê emrê jêrîn bikar bînin, û ziman, devera demjimêr û hwd dê bixweber bêne danîn.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo IPV6 çalak dike

IPV6 çalak bike da ku SMTP jî bi navnîşanên IPV6 e-nameyê bişîne.

biguherîne `/etc/sysctl.conf`

Rêzên jêrîn biguherînin an lê zêde bikin

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Bi [dersa contabo re bişopînin: Têkiliya IPv6 li servera xwe zêde bikin](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Biguherîne `/etc/netplan/01-netcfg.yaml` , çend rêzan lê zêde bike wekî ku di jimareya jêrîn de tê xuyang kirin (Dosya veavakirina xwerû ya Contabo VPS jixwe van rêzan hene, tenê wan şîrove nekin).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Dûv re `netplan apply` da ku veavakirina guhêrbar bikeve bandorê.

Piştî ku veavakirin serketî bû, hûn dikarin `curl 6.ipw.cn` bikar bînin da ku navnîşana ipv6 ya tora xweya derveyî bibînin.

## Vebijarkên depoya veavakirinê klon bikin

```
git clone https://github.com/wactax/ops.soft.git
```

## Ji bo navê domaina xwe sertîfîkayek SSL-ya belaş biafirînin

Ji bo şîfrekirin û îmzekirinê ji bo şandina nameyê belgeyek SSL hewce dike.

Em [acme.sh](https://github.com/acmesh-official/acme.sh) bikar tînin ku sertîfîkayan çêbikin.

acme.sh amûrek îmzekirina sertîfîkayê ya otomatîkî ya çavkaniyek vekirî ye,

Têkeve embara veavakirinê ops.soft, `./ssl.sh` bimeşîne, û peldankek `conf` dê di **pelrêça jorîn** de were çêkirin.

Pêşkêşvanê DNS-ya xwe ji [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) bibînin, `conf/conf.sh` biguherînin.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Dûv re `./ssl.sh 123.com` bimeşînin da ku ji bo navê domaina xwe sertîfîkayên `123.com` û `*.123.com` çêbikin.

Rêvekirina yekem dê bixweber [acme.sh](https://github.com/acmesh-official/acme.sh) saz bike û ji bo nûvekirina otomatîkî peywirek plansazkirî lê zêde bike. Hûn dikarin `crontab -l` bibînin, rêzek weha heye.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Rêya sertîfîkaya çêkirî tiştek wekî `/mnt/www/.acme.sh/123.com_ecc。`

Nûvekirina sertîfîkayê dê gazî skrîpta `conf/reload/123.com.sh` bike, vê skrîptê biguherîne, hûn dikarin fermanên wekî `nginx -s reload` zêde bikin da ku kaşa sertîfîkayê ya serîlêdanên têkildar nûve bikin.

## Pêşkêşkara SMTP bi chasquid ava bikin

[chasquid](https://github.com/albertito/chasquid) serverek SMTP ya çavkaniyek vekirî ye ku bi zimanê Go hatî nivîsandin.

Wekî cîgirek ji bo bernameyên servera nameyê ya kevnar ên wekî Postfix û Sendmail, chasquid hêsantir û hêsantir e ku bikar bîne, û ew ji bo pêşkeftina duyemîn jî hêsantir e.

Run `./chasquid/init.sh 123.com` dê bixweber bi yek klîk were saz kirin (li şûna 123.com navê domaina xweya şandinê bigire).

## Îmzeya E-nameyê DKIM mîheng bike

DKIM ji bo şandina îmzeyên e-nameyê tê bikar anîn da ku pêşî li nameyan wekî spam neyê girtin.

Piştî ku ferman bi serfirazî dimeşe, dê ji we were xwestin ku hûn qeyda DKIM (wek ku li jêr tê xuyang kirin) saz bikin.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Tenê tomarek TXT li DNS-ya xwe zêde bikin (wek ku li jêr tê xuyang kirin).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Rewş û têketinên karûbarê bibînin

 `systemctl status chasquid` Rewşa karûbarê bibînin.

Rewşa xebata normal wekî ku di wêneya jêrîn de tê xuyang kirin

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` an `journalctl -xeu chasquid` dikare têketina xeletiyê bibîne.

## Veavakirina navê domainê berevajî bike

Navê domainê berevajî ew e ku destûr bide navnîşana IP-yê ku bi navê domainê têkildar re were çareser kirin.

Sazkirina navek domaina berevajî dikare pêşî li naskirina e-name wekî spam bigire.

Dema ku e-name tê wergirtin, servera wergir dê li ser navnîşana IP-ya servera şandinê analîza navê domainê berevajî bike da ku piştrast bike ka servera şandî navek domaina berevajî ya derbasdar heye an na.

Ger servera dişîne navek domaina berevajî tune be an jî navnîşa domaina berevajî bi navnîşana IP-ya servera şandinê re nebe, dibe ku servera wergir e-nameyê wekî spam nas bike an jî red bike.

Serdana [https://my.contabo.com/rdns](https://my.contabo.com/rdns) bikin û wekî ku li jêr tê nîşandan mîheng bikin

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Piştî danîna navê domainê berevajî, ji bîr mekin ku çareseriya pêşwext ya navê domainê ipv4 û ipv6 ji serverê re mîheng bikin.

## Navê mêvandarê chasquid.conf biguherîne

`conf/chasquid/chasquid.conf` bi nirxa navê domainê berevajî biguherîne.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Dûv re `systemctl restart chasquid` da ku karûbarê ji nû ve bidin destpêkirin.

## Ji depoya git-ê re konfigurasyona paşvekişandinê

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Mînakî, ez peldanka conf-ê li pêvajoya github-a xwe wekî jêrîn vedigerim

Pêşî depoyek taybet biafirînin

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Têkeve pelrêça conf-ê û bişînin wargehê

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Şander zêde bike

rev

```
chasquid-util user-add i@wac.tax
```

Dikare şander lê zêde bike

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Piştrast bike ku şîfre rast hatiye danîn

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Piştî ku bikarhêner lê zêde kirin, `chasquid/domains/wac.tax/users` dê bêne nûve kirin, ji bîr mekin ku wê bişînin depoyê.

## DNS qeyda SPF zêde dike

SPF (Çarçoveya Siyaseta Sender) teknolojiyek verastkirina e-nameyê ye ku ji bo pêşîgirtina xapandina e-nameyê tête bikar anîn.

Ew bi kontrolkirina ku navnîşana IP-ya şanderê bi tomarên DNS-ê yên navê domainê ku ew îdîa dike ku ew e, nasnama şanderek e-nameyê rast dike, pêşî li şandina e-nameyên sextekaran digire.

Zêdekirina tomarên SPF-ê dikare bi qasî ku pêkan e-name wekî spam were nas kirin asteng bike.

Ger servera navê domaina we celebê SPF-ê piştgirî nake, tenê tomara tîpa TXT lê zêde bike.

Mînakî, SPF ya `wac.tax` wiha ye

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF ji bo `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Bala xwe bidinê ku min li vir `include:_spf.google.com` , ji ber ku ez ê paşê `i@wac.tax` wekî navnîşana şandinê di qutiya posta Google de mîheng bikim.

## Veavakirina DNS DMARC

DMARC kurteya (Rastkirina Peyama-based Domain, Raporkirin & Hevgirtin) ye.

Ew ji bo girtina paşnavên SPF-ê tê bikar anîn (dibe ku ji ber xeletiyên mîhengê çêbibe, an jî kesek din xwe wekî hûn spam dişîne).

Tomara TXT `_dmarc` zêde bike,

Naverok wiha ye

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Wateya her parameterê wiha ye

### p (Siyaset)

Nîşan dide ka meriv çawa e-nameyên ku verastkirina SPF (Çarçoveya Siyaseta Bişîne) an DKIM (Peyala Nasnameya DomainKeys) têk naçe, bi rê ve dibe. Parametreya p dikare li yek ji sê nirxan were danîn:

* tune: Tiştek nayê girtin, tenê encama verastkirinê bi mekanîzmaya raporkirina e-nameyê ji şanderê re tê vegerandin.
* Quarantîne: E-nameya ku ji verastkirinê derbaz nebûye têxin peldanka spam, lê dê rasterast e-nameyê red neke.
* redkirin: E-nameyên ku verastkirinê têk diçin rasterast red bikin.

### fo (Vebijarkên têkçûn)

Hejmara agahdariya ku ji hêla mekanîzmaya raporê ve hatî vegerandin diyar dike. Ew dikare li yek ji nirxên jêrîn were danîn:

* 0: Encamên pejirandinê ji bo hemî peyaman rapor bikin
* 1: Tenê peyamên ku verastkirinê têk diçin rapor bikin
* d: Tenê têkçûnên verastkirina navê domainê rapor bikin
* s: tenê têkçûnên verastkirina SPF rapor bikin
* l: Tenê têkçûnên verastkirina DKIM rapor bikin

### rua & ruf

* rua (Raporkirina URI ji bo raporên Tevhev): Navnîşana e-nameyê ji bo wergirtina raporên hevgirtî
* ruf (Raporkirina URI ji bo raporên Dadwerî): navnîşana e-nameyê ji bo wergirtina raporên berfireh

## Tomarên MX-ê zêde bikin ku e-nameyên ji Google Mail re bişînin

Ji ber ku min nekarî qutiyek posteya pargîdanî ya belaş bibînim ku navnîşanên gerdûnî piştgirî dike (Catch-All, dikare her e-nameyên ku ji vê navê domainê re hatine şandin, bêyî sînorkirinên pêşgiran werbigire), min chasquid bikar anî da ku hemî e-name bişîne qutiya posta xwe ya Gmail.

**Ger qutiya posta karsaziya weya xweya dravî heye, ji kerema xwe MX-ê neguhezînin û vê gavê berdin.**

`conf/chasquid/domains/wac.tax/aliases` biguherîne, qutiya posta şandinê saz bike

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` hemî e-nameyan destnîşan dike, `i` pêşgira navnîşana e-nameyê ya bikarhênerê şandî ye ku li jor hatî afirandin. Ji bo şandina nameyê, her bikarhêner pêdivî ye ku rêzek lê zêde bike.

Dûv re tomara MX-ê lê zêde bike (ez rasterast navnîşana navnîşa navnîşa berevajî ya vir destnîşan dikim, wekî ku di rêza yekem de di wêneya jêrîn de tê xuyang kirin).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Piştî ku veavakirin qediya, hûn dikarin navnîşanên e-nameyên din bikar bînin da ku e-nameyên bişînin `i@wac.tax` û `any123@wac.tax` da ku bibînin ka hûn dikarin e-nameyên Gmail-ê bistînin.

Heke na, têketina chasquid ( `grep chasquid /var/log/syslog` ) kontrol bikin.

## Bi Google Mail re e-nameyek ji i@wac.tax re bişînin

Piştî ku Google Mail e-name wergirt, min bi xwezayî hêvî kir ku li şûna i.wac.tax@gmail.com bi `i@wac.tax` bersiv bidim.

Serdana [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) bikin û bikirtînin "Navnîşana e-nameyek din lê zêde bike".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Dûv re, koda verastkirinê ku ji hêla e-nameya ku jê re hatî şandin ve hatî wergirtin têkevin.

Di dawiyê de, ew dikare wekî navnîşana şanderê xwerû (ligel vebijarka ku bi heman navnîşanê bersiv bide) were danîn.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Bi vî awayî, me sazkirina servera posta SMTP qedandiye û di heman demê de Google Mail bikar tîne ji bo şandin û wergirtina e-nameyê.

## E-nameyek ceribandinê bişînin da ku kontrol bikin ka veavakirin serketî ye

Bikeve `ops/chasquid`

`direnv allow` bide sazkirina girêdanan (direnv di pêvajoya destpêkirina yek-kilît a berê de hatîye saz kirin û çengek li şêlê hatîye zêdekirin)

paşê birevin

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Wateya pîvanan wiha ye

* bikarhêner: navê bikarhêner SMTP
* derbas: şîfreya SMTP
* ber: wergir

Hûn dikarin e-nameyek testê bişînin.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Tête pêşniyar kirin ku Gmail bikar bînin da ku e-nameyên ceribandinê bistînin da ku kontrol bikin ka veavakirin serketî ne.

### Şîfrekirina standard TLS

Wekî ku di jimareya jêrîn de tê xuyang kirin, ev kilîtka piçûk heye, ku tê vê wateyê ku sertîfîkaya SSL bi serfirazî hate çalak kirin.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Dûv re bikirtînin "E-nameya Orjînal Nîşan Bide"

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Wekî ku di jimareya jêrîn de tê xuyang kirin, rûpela e-nameya orîjînal a Gmail DKIM nîşan dide, ku tê vê wateyê ku veavakirina DKIM serketî ye.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Di sernavê e-nameya orjînal de Received binihêrin, û hûn dikarin bibînin ku navnîşana şanderê IPV6 e, ku tê vê wateyê ku IPV6 jî bi serfirazî hatî mîheng kirin.
