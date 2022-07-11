l!/bin/bash<br>

#ftpHost="m122-server.local"<br>
#ftpUser="IoT-Log"<br>
#ftpPass="THLS-60s"<br>
#ftpDir="/log/data_sound.txt"<br>
#localDir="home"<br>

#ftp -inv  $ftpHost << FTPANWEISUNG<br>

#quote USER $ftpUser<br>
#quote PASS $ftpPass<br>

#passive<br>
#ascii<br>

#cd $ftpDir<br>
#lcd $localDir<br>
#mget *.txt<br>

#lcd ..<br>
#cd ..<br>

#close<br>

#FTPANWEISUNG<br>

#Obiges ist was eine FTP verbindung sein sollte jedoch hat jene nie bei mir funktionierete<br>


touch lautstärke.txt |exit<br>
#Ist eine datei in welcher schön dargestellt eine ausgabe steht von der aktuellsten Audio aufnahme<br>

echo "Datum der Angaben:" >> lautstärke.txt #Der Übertitel des Datums<br>
cat data_sound.txt | tail -n1 | cut -d ',' -f3 | tee >> lautstärke.txt; #-f3 verweist hier auf die <br>tabelle wo das Datum aufgelistet steht<br>

echo "Zeit der Angaben:" >> lautstärke.txt #Der Übertitel der Zeit<br>
cat data_sound.txt | tail -n1 | cut -d ',' -f4 | tee >> lautstärke.txt; #-f3 verweist hier auf die <br>tabelle wo die Zeit aufgelistet steht<br>

echo "Lautstärke:" >> lautstärke.txt #Der Übertitel der Lautstärken messung<br>
cat data_sound.txt | tail -n1 | cut -d ',' -f2 | tee >> lautstärke.txt; #-f3 verweist hier auf die <br>tabelle wo die lautstärken Messung hinterlegt ist<br>


touch zwischen.txt<br>
cat data_sound.txt | tail -n1 | cut -d ',' -f2 | bc > zwichen.txt #liest den lautstärkewert aus und <br>fügt ihn in eine weitere Datei (zwischen.txt).<br>

konvertieren=$(cat zwischen.txt | bc) #Eine variabel als platzhalter genutzt um in der unteren <br>variabel (zulaut) die "396" Zahl zu einer 396 Zahl, also Integer, zu formen. <br>
zulaut=$(echo "$(($konvertieren + 0))") #Formt zu Integer<br>

##Obiges ist eine funktionierende umwandlung von den lautstärken Messungen. Da die lautstärke ><br>
##Genutz hätte ich die umvormatierten Dateien um ein If einzubauen welches ich nutzen hätte kö><br>


touch zulaut.txt<br>
touch normal.txt<br>

if [ "$zulaut" -ge 500 ] #nimt aus der variabel zulaut die werte und vergleicht ob die lautstär><br>
        then<br>
        cat data_sound.txt | grep $zulaut | tee >> zulaut.txt #Schiebt die zu laute Daten in ei><br>
else<br>
        cat data_sound.txt | grep $zulaut | tee >> normal.txt #Schiebt die nicht zu lauten Date><br>
fi<br>
