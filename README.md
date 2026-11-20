# Brooklyn-Nine-Nine-CTF

Nesse Lab consistia em CTF de OSINT, nele nẫo tinha muita coisa mas tinha algumas dicas.

<img width="1120" height="194" alt="Screenshot From 2026-11-20 11-51-51" src="https://github.com/user-attachments/assets/decb03e8-d119-4690-8d62-7dfe243ce6a1" />


*Recon*
Primeiro liguei o servidor e peguei o IP,após isso verifiquei se estava retornado algo com o <ping -c 4 10.112.190.82> e aguardei retornar os 100% receive
após isso eu coloquei no gobuster e no nmap, enquanto ele rodavam eu joguei o IP na URL.

no nmap <nmap -sV -sC -T4 10.112.190.82>

no gobuster <gobuster dir  -u http://10.112.190.82/ -w /usr/share/wordlists/common.txt>

<img width="1869" height="983" alt="Screenshot From 2026-11-20 11-50-34" src="https://github.com/user-attachments/assets/3160b168-1993-4606-8557-25d01ae2dc7c" />

tinha essa mensagem no final da tela, dai eu baixei a imagem do brooklyn e tentei achar alguma coisa com o string.

<img width="945" height="930" alt="Screenshot From 2026-11-20 10-48-44" src="https://github.com/user-attachments/assets/267dc004-6e9a-45d3-839e-ab1183568dad" />

pensei que acharia alguma coisa com isso, mas não achei nada de útil, dai fui ler o código fonte da pagina.

<img width="1869" height="983" alt="Screenshot From 2026-11-20 11-51-15" src="https://github.com/user-attachments/assets/295c5c09-87fa-4655-9e20-2cf34dbd9adc" />

com a pergunta que tinha, eu fui correr atrás disso com esse sites:

https://www.georgeom.net/StegOnline/image
nesse ele basicamente olha alguma coisa envolvendo as cores, mas la tem um passo a passo de como procurar isso envolvendo CTF, lá eu não achei nada que me ajudasse além das strings que eu já tinha visto.

https://exif.tools/
esse foi para ver os metadados da imagem, que continha apena alguns dados que eu não sabia com utilizar, procurei e realmente não eram úteis.

depois disso tudo olhai os resultados do nmap e do gobuster, apenas o nmap retornou algo util.

<img width="904" height="663" alt="Screenshot From 2026-11-20 10-49-33" src="https://github.com/user-attachments/assets/958aa3a6-8bcb-4e9e-8a7d-825655d7f156" />

dai ele retornou um server FTP que tem uma mensagem para o jake, ai tentei acessar o sever.

<img width="918" height="765" alt="Screenshot From 2026-11-20 10-48-22" src="https://github.com/user-attachments/assets/c33944a6-1038-4179-934d-a67f40b56c6d" />

e consegui entrar no server, e nisso eu dei um <ls> e depois dei  <get note_to_jake.txt> para baixar ele e sai com <quit>.

nesse arquivo tem uma mensagem :

<img width="784" height="102" alt="image" src="https://github.com/user-attachments/assets/df65d6ac-3c59-4862-9f23-9941768b2b28" />

nessa nos ja sabemos o user e precisamos da senha, então eu usei o hydra para achar essa senha.

<img width="968" height="988" alt="Screenshot From 2026-11-20 11-20-58" src="https://github.com/user-attachments/assets/3de60019-55a3-450b-8cfb-e7757371350a" />

e achamos a senha que aparentemente e muito fraca, nessa foi so fazer o acesso ssh procurar o arquivo que la estaria a 1 flag

<img width="349" height="46" alt="Screenshot From 2026-11-20 11-47-17" src="https://github.com/user-attachments/assets/728eac22-5092-46dd-a028-c9ab20a86b09" />

após isso foi so entra com <sudo less /usr/profile> 

<img width="959" height="649" alt="Screenshot From 2026-11-20 11-48-26" src="https://github.com/user-attachments/assets/1567c9e9-f51a-4088-b592-bec100bffc52" />

e apos isso dar um <!/bin/sh> ao final do arquivo

<img width="959" height="919" alt="Screenshot From 2026-11-20 11-48-42" src="https://github.com/user-attachments/assets/bc7c98da-cb38-49f9-939e-f05eaf58b5b6" />

apos voce entrar e so dar um <id> e a segunda flag aparece.


