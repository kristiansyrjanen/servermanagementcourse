#Harjoitus 5

##a) Valitse aihe omaksi kurssityöksi ja varaa se kommenttina aikataulusivun perään.

Valitsin moduulin aiheeksi Teamspeak 3 VoIP palvelimen asennuksen salttia hyödyntäen.


##b) Julkaise raportti MarkDownilla. Toteutuu c) kohdassa.

##c) Aja oma Salt-tila suoraan git-varastosta.

Harjoitus tehty Lenovo Ideapad 700 kannettavalla, jossa Kubuntu 18.04 LTS.

###Asennetaan sekä konfiguroidaan Salt-Master sekä minion.

Aloitetaan päivittämällä paketit
	sudo apt-get update && upgrade

Sitten asennamme Salt-masterin sekä minionin.
	sudo apt-get install salt-master salt-master

Lisäämme masterille minionin.
	sudoedit /etc/salt/minion
	master: [IP-osoite]
	id: test1

Poistamme file_ignore_glob varoituksen salt-masterilta
	sudoedit /etc/salt/master
	file_ignore_glob: []

Sitten käynnistämme masterin sekä minionin jotta muutokset tulevat voimaan.
	sudo systemctl restart salt-minion.service
	sudo systemctl restart salt-master.service

Hyväksymme minionin avaimen,
	~$ sudo salt-key -A
	The following keys are going to be accepted:
	Unaccepted Keys:
	test1
	Proceed? [n/Y] y
	Key for minion test1 accepted.

###Kloonataan tila git-varastosta ja ajetaan se.

Kloonataan tila omasta varastosta komennolla
	git clone https://github.com/kristiansyrjanen/teststate.git

Mennään hakemistoon teststate ja siirretään se /srv/ hakemiston alle.
	cd teststate
	sudo mv salt/ /srv/

Seuraavaksi ajetaan tila.
	sudo salt '*' state.highstate

	test1:
	----------
	          ID: gedit
	    Function: pkg.installed
	      Result: True
	     Comment: The following packages were installed/updated: gedit
	     Started: 21:10:26.823191
	    Duration: 28107.773 ms
	     Changes:   
	              ----------
	              gedit:
	                  ----------
	                  new:
	                      3.28.1-1ubuntu1
	                  old:
	              gedit-common:
	                  ----------
	                  new:
	                      3.28.1-1ubuntu1
	                  old:
	              gir1.2-gtksource-3.0:
	                  ----------
	                  new:
	                      3.24.7-1
	                  old:
	              gir1.2-peas-1.0:
	                  ----------
	                  new:
	                      1.22.0-2
	                  old:
	              gir1.2-peasgtk-1.0:
	                  ----------
	                  new:
	                      1
	                  old:
	              libgspell-1-1:
	                  ----------
	                  new:
	                      1.6.1-1
	                  old:
	              libgspell-1-common:
	                  ----------
	                  new:
	                      1.6.1-1
	                  old:
	              libgtksourceview-3.0-1:
	                  ----------
	                  new:
	                      3.24.7-1
	                  old:
	              libgtksourceview-3.0-common:
	                  ----------
	                  new:
	                      3.24.7-1
	                  old:
	              libjavascriptcoregtk-4.0-18:
	                  ----------
	                  new:
	                      2.20.1-1
	                  old:
	              libpeas-1.0-0:
	                  ----------
	                  new:
	                      1.22.0-2
	                  old:
	              libpeas-1.0-0-python3loader:
	                  ----------
	                  new:
	                      1
	                  old:
	              libpeas-common:
	                  ----------
	                  new:
	                      1.22.0-2
	                  old:
	              libwebkit2gtk-4.0-37:
	                  ----------
	                  new:
	                      2.20.1-1
	                  old:
	              libyelp0:
	                  ----------
	                  new:
	                      3.26.0-1ubuntu2
	                  old:
	              python3-gi-cairo:
	                  ----------
	                  new:
	                      3.26.1-2
	                  old:
	              yelp:
	                  ----------
	                  new:
	                      3.26.0-1ubuntu2
	                  old:
	              yelp-xsl:
	                  ----------
	                  new:
	                      3.20.1-4
	                  old:
	              zenity:
	                  ----------
	                  new:
	                      3.28.1-1
	                  old:
	              zenity-common:
	                  ----------
	                  new:
	                      3.28.1-1
	                  old:
	----------
	          ID: vlc
	    Function: pkg.installed
	      Result: True
	     Comment: All specified packages are already installed
	     Started: 21:10:54.934875
	    Duration: 1425.284 ms
	     Changes:   
	
	Summary for test1
	------------
	Succeeded: 2 (changed=1)
	Failed:    0
	------------
	Total states run:     2
	Total run time:  29.533 s

Tila palaa onnistuneesti.


Lähteet:
http://terokarvinen.com/2018/aikataulu-%e2%80%93-palvelinten-hallinta-ict4tn022-4-ti-5-ke-5-loppukevat-2018-5p
https://github.com/kristiansyrjanen/teststate.git
