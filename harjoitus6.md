# Palvelinten Hallinta Harjoitus 6
[Palvelinten Hallinta kurssin harjoitukset Tero Karvisen opastamana.](http://terokarvinen.com/2018/aikataulu-%E2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p)
Harjoitus suoritettu Lenovo Ideapad 700:lla, Xubuntu 18.04 LTS (amd64) Livetikulla.

## [a) Kultainen polku. Tee ensimmäinen versio modulistasi, joka toimii ainakin optimiolosuhteissa. ](http://terokarvinen.com/2018/aikataulu-%E2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p)

Olen työstänyt moduulia noin viikon ajan ja se löytyy [GitHub sivuiltani](https://github.com/kristiansyrjanen/teamspeak3-salted)

Ensimmäinen toimiva versioni oli Xubuntu 16.04 Livetikulle joka Salt-tilaa käyttäen asentaa TeamSpeak 3 VoIP palvelimen. Tärkeänä tekijänä tuossa kyseisessä versiossa oli se että se toimi ainoastaan xubuntu nimisellä käyttäjällä. Tällä hetkellä kuitenkin yritän saada tilaa toimimaan millä nimellä tahansa. 



## [b) Kokeile moduliasi tyhjässä koneessa. Voit käyttää virtualboxia, vagranttia tai livetikkua.](http://terokarvinen.com/2018/aikataulu-%E2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p)
Kokeilen Lenovo Ideapad 700 kannettavallani, Xubuntu 18.04 LTS(amd64) Livetikulla.

### Valmistellaan kannettava

Avattuani kannettavan vaihdan samantien Suomi-näppäimistön käyttöön ja kirjaudun WiFi verkkooni.

    setxkbmap fi

Saatuani yhteyden verkkoon päivitän paketit ja asennan tarvittavat paketit.

    sudo apt-get update
    $...
    sudo apt-get -y install salt-master salt-minion git

Yhdistän Salt-Minionin Salt-Masteriin ja poistan samalla **file_ignore_glob:** virheilmoituksen

    sudoedit /etc/salt/minion
    $
    master: [IP-osoite]
    id: teamspeak
    
    sudoedit /etc/salt/master
    $
    file_ignore_glob: []

Tämän jälkeen joudumme uudelleenkäynnistämään demonit jotta muutokset tulevat voimaan.

    sudo systemctl restart salt-master.service
    sudo systemctl restart salt-minion.service
    
Tarkastetaan että olemme saaneet uuden avaimen **Salt-Masterille** hyväksyttäväksi ja hyväksytään se **Salt-Minioniksi**

    sudo salt-key
    $
    Accepted Keys:
    Denied Keys:
    Unaccepted Keys:
    teamspeak
    Rejected Keys:
    
    sudo salt-key -A
    The following keys are going to be accepted:
    Unaccepted Keys:
    teamspeak
    Proceed? [n/Y] y
    Key for minion teamspeak accepted.

Kokeillaan että saamme yhteyden Salt-Minioniin

    sudo salt '*' test.ping
    $
    teamspeak:
        True

Seuraavaksi olemme valmiita ajamaan tila Saltilla, ensin tarvitsemme itse tilan.

### Salt-tilan kloonaus ja sijoitus oikeaan paikkaan

Kloonaan Salt-tilani GitHubista

    git clone https://github.com/kristiansyrjanen/teamspeak3-salted.git

Tämän jälkeen menen kyseiseen kansioon ja siirrän **salt** kansion **/srv/** hakemiston alle

    cd /home/xubuntu/teamspeak3-salted
    sudo mv salt/ /srv/

Ajetaan Salt-tila Minionilla.

    sudo salt 'teamspeak' state.highstate

*Tila näyttää siltä että se ei suoriutuisi mutta tosiasiassa Teamspeak 3 palvelin on pystyssä ja siihen voi yhdistäytyä. Scriptini ei vain suostu sulkemaan/poistumaan "ts3serve_startscript.sh" ruudusta.*

KORJATTU, kirjoitusvirhe bash-scriptissä joka ei yhdistänyt **ts3serve_startscript**-scriptiä **/etc/init.d/teamspeak**:n. Joten Teamspeak 3 demoni käynnistetään aivan niinkuin mikä tahansa demoni...

        sudo systemctl start teamspeak 


Palvelin toimii.
![TS3 Palvelin pystyssä](https://i.imgur.com/fRmCWqC.png)


## [c) Käyttäjätarina (user story): ketkä ovat modulisi käyttäjät? Mitä he haluavat saada aikaan modulillasi? Missä tilanteessa he sitä käyttävät? Mitkä ovat tärkeimmät parannukset käyttäjän kannalta, joita moduliin pitäisi vielä tehdä?](http://terokarvinen.com/2018/aikataulu-%E2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p)

Moduulini sopii kenelle tahansa oman VoIP palvelimen tarvitsevalle. Loin skriptin joka tekee koko asennuksen käyttäjän puolesta. Se on nopea tapa saada VoIP palvelua tarvitsevalle itse ylläpidetty versio pystyyn. Kätevä vaikkapa LAN-turnauksen järjestäjälle. Moduuli pitäisi saada tehtyä siten että se toimisi muulla tavalla kuin Livetikulla... Tai toimiihan tila perjaatteessa missä vain jos käyttäjä osaa itse vaihtaa tarvittavat kohdat skriptistä.


References: 

[https://github.com/kristiansyrjanen](https://github.com/kristiansyrjanen)

[TeroKarvinen.com - Palvelinten Hallinta](http://terokarvinen.com/2018/aikataulu-%E2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p)
