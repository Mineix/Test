# Display für Hauptmenü
option=$(dialog --menu 'Nutzer und Gruppen Verwaltung' 0 0 0 \
  1 'Add user' \
  2 'Edit user' \
  3 'Delete user' \
  4 'Add group' \
  5 'Edit group' \
  6 'Delete group' \
  7 'List users' \
  8 'List groups' \
  9 'Exit' \
  3>&1 1>&2 2>&3)

# Überprüft des Atwort des Nutzer
case $option in
  1)
    # Nutzer erstellen

    username=$(kdialog --inputbox 'Schreiben sie bitte Name der Nutzer die Sie  erstellen möchten ' 0 0)


  if [ -z "$username" ]; then
  kdialog --error "Bitte geben Sie einen gültigen Nutzernamen an!" 80 80
  exit 1
  fi
        if ! [[ "$username" =~ ^[a-zA-Z0-9]+$ ]]; then
        kdialog --error "Der Nutzername darf nur alphanumerische Zeichen enthalten und darf keine Leerzeichen haben!" 80 80
        exit 1
        fi
        	if [ $(grep -c -i "^${username}:" /etc/passwd ) -eq 1  ]; then
        	# Würde eine fehler gegeben das User ist bereits vorhanden!
        	kdialog --error "Name ist bereits vorhanden!" 80 80
        	exit 1





    else
    # User hinzufügen
    useradd "$username"
    (kdialog --msgbox  " User   $username  hinzugefügt!" 80 80)
    exit 1
    fi
   ;;
  2)
    # Nutzer ändern
    username=$(kdialog --inputbox 'Schreiben sie bitte Name der Nutzer die Sie ändern möchten ' 0 0)
    # "usermod"
    if [ -z "$username" ]; then
        kdialog --error "Bitte geben Sie einen gültigen Nutzernamen an!" 80 80
   fi
    	if [ $(grep -c -i "^${username}:" /etc/passwd ) -eq 0  ]; then
        	# Fehler falls es gibt keine Username mit gleichem name
        	kdialog --error "Es gibt keine Username mit diese Name!" 80 80
        	exit 1
   	 else
   		 usermod "$username"
    		  (kdialog --msgbox  " Geschaft!" 80 80)
   		 exit 0
    		fi
    		;;
  3)
    # Nutzer löshen
  username=$(kdialog --inputbox 'Schreiben sie bitte Name der Nutzer die Sie löshen möchten:' 0 0)

    #Überprüft ob User mit diese Name gibts
    if [ $(grep -c -i "^${username}:" /etc/passwd ) -eq 0  ]; then
    # Zeigt Fehlermeldung fall User mit diesem Namen gibts nicht!
    kdialog --error "User mit diesem Namen gibts nicht!" 80 80
    exit 1
    else
    #User löschen
    userdel "$username"
     (kdialog --msgbox  " Username:    $username  gelöscht!" 80 80)
    fi
    ;;

  4)

     groupname=$(kdialog --inputbox 'Schreiben sie bitte Name der neue Gruppe:' 0 0)

    if ! [[ "$groupname" =~ ^[a-zA-Z0-9]+$ ]]; then
        kdialog --error "Die Gruppename darf nur alphanumerische Zeichen enthalten und darf keine Leerzeichen haben!" 80 80
        exit 1
        fi
     if [ -z "$groupname" ]; then
        kdialog --error "Bitte geben Sie einen gültigen Gruppenamen an!" 80 80
        exit 1
        fi
    if [ $(grep -c -i "^${groupname}:" /etc/group) -eq 1 ]; then
      # Fehler wenn es eine gruppe mit gleichem Name schon vorhanden!
      kdialog --error "Gruppe schon vorhanden!" 80 80
      exit 1
    else
      # 
      groupadd "$groupname"
      # Nachricht dass alles klappt
      (kdialog --msgbox  " Gruppe    $groupname  hinzugefügt!" 80 80)
      exit 0
    fi
    ;;

  5)
    # Gruppe ändern
    groupname=$(kdialog --inputbox 'Schreiben sie bitte Name der Gruppe die Sie ändern möchten:' 0 0)
    if [ -z "$groupname" ]; then
        kdialog --error "Bitte geben Sie einen gültigen Gruppenamen an!" 80 80
        exit 1
        fi
    if [ $(grep -c -i "^${groupname}:" /etc/group) -eq 0 ]; then
      # Display an error message if the group already exists
      kdialog --error "Gruppe gibts nicht" 80 80
      exit 1
    else
    # "groupmod"
    groupmod "$groupname"
    (kdialog --msgbox  " Gruppe:   $groupname  geändert!" 80 80)
    fi
   
    ;;
  6)
    # Gruppe löschen
    groupname=$(kdialog --inputbox 'Schreiben sie bitte Name der Gruppe die Sie löshen möchten:' 0 0)

        if [ -z "$groupname" ]; then
        kdialog --error "Bitte geben Sie einen gültigen Gruppenamen an!" 80 80
        exit 1
        fi
    if [ $(grep -c -i "^${groupname}:" /etc/group) -eq 0 ]; then
      # Display an error message if the group already exists
      kdialog --error "Gruppe gibts nicht" 80 80
      exit 1
    else


    # "groupdel"
    groupdel "$groupname"
    (kdialog --msgbox  " Gruppe:   $groupname  gelöscht!" 80 80)
    exit 0
    fi
    ;;
  7)
    # Liste von Nutzer
    # "cut" und "awk"
    cut -d: -f1 /etc/passwd | awk '{print $1}'
    ;;

 8) ## Liste von Gruppen
    #  "cut" und "awk"
    cut -d: -f1 /etc/group | awk '{print $1}'
    ;;
 9) clear
    exit 0
    ;;

 *)
 clear
 exit 0
;;
esac
Footer
© 2023 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
