# material for Romain

This is where Romain can find all the informations for his internship with Emmanuel.

 - have a look at : https://github.com/meom-group/tutos
 - create your github account
 - learn git, conda, jupyter
 - learn about cal1
 - install conda + jupyter
 - learn about NEMO outputs + ensemble + ARGO profiles
 - run the example notebook
 - modify it at your will


About the data, Jean-Marc info :

Les données OBS du run ORCA025.L75-OCCITENS  (run d'ensemble OCCIPUT) sont sous forme de fichiers tar (gz) dans

/mnt/meom/MODEL_SET/ORCA025.L75/ORCA025.L75-OCCITENS/OBS

Pour Romain, on s'interesse aux données 'enact'  , fichiers de type OBS-enact_y1977_12m.tgz   ou OBS-enact_y1977_1y.tgz
Chaque fichier correspond a une année.
Dans les fichiers 1y on 50 fichiers annuels correspondant aux 50 membres
Dans les fichiers 12m on 50 x 12 fichiers mensuels correspondant aux 50 membres et 12 mois de l'année

Par exemple dans OBS-enact_y1977_1y.tgz il y a :
OBS-enact_y1978/
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.001_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.002_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.003_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.004_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.005_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.006_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.007_y2005_enact_fdbk.nc  --> membre 007
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.008_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.009_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.010_y2005_enact_fdbk.nc
......
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.043_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.044_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.045_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.046_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.047_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.048_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.049_y2005_enact_fdbk.nc
-rw-r--r-- 1 molinesj domainusers 2.1G Jul  9  2016 ORCA025.L75-OCCITENS.050_y2005_enact_fdbk.nc
....

Le numero du membre de 001 a 050  est le chiffre apres OCCITENS.

Ces fichiers sont des netcdf avec la convention 'feedback (fdbk)' Ils contiennent beaucoup de variables ...(J'ai détarré le fichier OBS-enact_y2005_1y.tgz pour exemple, dans OBS-enact_y2005 )

Les dimensions importantes sont N_OBS et N_LEVELS donnant le nombre de profils dans le fichier, et le nombre de niveaux max par profils.
Les variables d´interets sont :

    double LONGITUDE(N_OBS) ;
    double LATITUDE(N_OBS) ;
    double DEPTH(N_OBS, N_LEVELS) ;
    double JULD(N_OBS) ;  # date en jours julien

Les variables physiques sont :
    float POTM_OBS(N_OBS, N_LEVELS) ;     Temperature potentielle observee
    float POTM_Hx(N_OBS, N_LEVELS) ;      Temperature potentielle simulee
    float PSAL_OBS(N_OBS, N_LEVELS) ;     Salinite observee (PSU)
    float PSAL_Hx(N_OBS, N_LEVELS) ;      Salinite simulee (PSU)
    float TEMP(N_OBS, N_LEVELS) ;         Temperature in situ observee (non simulee)

Il y a ensuite un tas de variable _QC ( qui sont des flags de qualite -- pour les observations--). Dans le modèle, on n'a pas tenu compte de ces flags de qualité et on a des profils modèle pour chaque profil observé, quelle que soit sa qualité.

Un détail important que je n'ai pas mentionné : une observation correspond a un profil.  Le meme flotteur ARGO fait bien sur plusieurs profils.... Si on veux par exemple tous les profils d'un flotteur ( qui se deplace donc...) il faut regarder la variable ( char)  STATION_IDENTIFIER(N_OBS)  et selectionner les profils ayant le bon IDENTIFIER.

PAr exemple, pour le STATION_IDENTIFIER "1589340 ", dans le fichier 2005 il y a 364  profils
( ncdump -v STATION_IDENTIFIER ORCA025.L75-OCCITENS.011_y2005_enact_fdbk.nc | grep 1589340 | wc  )

Remarque : ces fichiers representent  la base de donnees ENACT-ENSEMBLE (et pas seulement ARGO).  ( ie il y a des mouillages )



