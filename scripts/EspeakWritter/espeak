#!/bin/bash
#script para escribir en espeak, hecho por Jebús Debliter ewe"
#instalara las dependencias en caso no la tengas
clear && echo "instalando dependencias" && sleep 1s
sudo apt install espeak mbrola-es1 && clear
while :
do
echo "Escriba texto: "
read -e texto
espeak -v mb-es1 -s150 -p50 "$texto" && clear
done
