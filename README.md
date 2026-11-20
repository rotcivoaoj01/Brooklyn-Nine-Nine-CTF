***

# Write-up: TryHackMe - Brooklyn Nine Nine

Este laboratório consistia em um desafio CTF focado em exploração de serviços e escalação de privilégios.

<img width="1120" height="194" alt="Screenshot From 2026-11-20 11-51-51" src="https://github.com/user-attachments/assets/decb03e8-d119-4690-8d62-7dfe243ce6a1" />

## 1. Reconhecimento (Recon)

Primeiro, liguei o servidor e peguei o endereço IP. Verifiquei a conectividade com um ping para garantir que os pacotes estavam chegando:

```bash
ping -c 4 10.112.190.82
```

Enquanto as ferramentas de varredura rodavam, também joguei o IP diretamente na URL do navegador.

*   **Varredura com Nmap:** `nmap -sV -sC -T4 10.112.190.82`
*   **Varredura de diretórios com Gobuster:** `gobuster dir -u http://10.112.190.82/ -w /usr/share/wordlists/common.txt`

<img width="1869" height="983" alt="Screenshot From 2026-11-20 11-50-34" src="https://github.com/user-attachments/assets/3160b168-1993-4606-8557-25d01ae2dc7c" />

Na página web, havia uma mensagem no final da tela. Baixei a imagem do Brooklyn e tentei achar algo usando o comando `strings`, mas não achei nada de útil.

<img width="945" height="930" alt="Screenshot From 2026-11-20 10-48-44" src="https://github.com/user-attachments/assets/267dc004-6e9a-45d3-839e-ab1183568dad" />

Decidi então ler o código-fonte da página, que continha uma pergunta. Fui atrás dessa pergunta usando ferramentas de esteganografia e metadados:

<img width="1869" height="983" alt="Screenshot From 2026-11-20 11-51-15" src="https://github.com/user-attachments/assets/295c5c09-87fa-4655-9e20-2cf34dbd9adc" />

*   **[StegOnline](https://www.georgeom.net/StegOnline/image):** Usei para olhar as cores, mas não achei nada além das strings que já tinha visto.
*   **[exif.tools](https://exif.tools/):** Usei para ver os metadados da imagem. Continha alguns dados, mas nada que fosse útil para o desafio.

Depois disso tudo, voltei a olhar os resultados do Nmap e do Gobuster. Apenas o **Nmap** retornou algo útil:

<img width="904" height="663" alt="Screenshot From 2026-11-20 10-49-33" src="https://github.com/user-attachments/assets/958aa3a6-8bcb-4e9e-8a7d-825655d7f156" />

## 2. Exploração (FTP)

O Nmap revelou um servidor **FTP** rodando e com login anônimo habilitado, contendo uma mensagem para o usuário "jake". 

Tentei acessar o servidor:

<img width="918" height="765" alt="Screenshot From 2026-11-20 10-48-22" src="https://github.com/user-attachments/assets/c33944a6-1038-4179-934d-a67f40b56c6d" />

Consegui entrar. Usei o comando `ls` para listar os arquivos, baixei o arquivo `note_to_jake.txt` com o `get` e saí com o `quit`.

O arquivo continha a seguinte mensagem:

<img width="784" height="102" alt="image" src="https://github.com/user-attachments/assets/df65d6ac-3c59-4862-9f23-9941768b2b28" />

Com essa mensagem, já sabíamos o nome de usuário, mas precisávamos da senha. Então, usei o **Hydra** para descobrir a senha via SSH:

```bash
hydra -l jake -P /usr/share/wordlists/rockyou.txt ssh://10.112.190.82
```

<img width="968" height="988" alt="Screenshot From 2026-11-20 11-20-58" src="https://github.com/user-attachments/assets/3de60019-55a3-450b-8cfb-e7757371350a" />

E achamos a senha: **`987654321`** (que é realmente muito fraca).

## 3. Acesso e Escalação de Privilégios

Com a senha em mãos, fiz o acesso via SSH para procurar o arquivo com a primeira flag:

```bash
ssh jake@10.112.190.82
```
A primeira Flag estado no caminho abaixo.

<img width="349" height="46" alt="Screenshot From 2026-11-20 11-47-17" src="https://github.com/user-attachments/assets/728eac22-5092-46dd-a028-c9ab20a86b09" />

Após isso, para escalar privilégios, usei o comando `sudo` combinado com o `less`:

```bash
sudo less /etc/profile
```
*(Nota: O correto é `/etc/profile`, e não `/usr/profile`).*

<img width="959" height="649" alt="Screenshot From 2026-11-20 11-48-26" src="https://github.com/user-attachments/assets/1567c9e9-f51a-4088-b592-bec100bffc52" />

Com o `less` aberto, digitei `!/bin/sh` no final do arquivo (na tela do visualizador) para spawnar uma shell como root:

<img width="959" height="919" alt="Screenshot From 2026-11-20 11-48-42" src="https://github.com/user-attachments/assets/bc7c98da-cb38-49f9-939e-f05eaf58b5b6" />

Após entrar na shell, basta dar um `id` para confirmar o acesso root e ler a segunda flag:

```bash
id
cat /root/root.txt
```

***
*(Basta copiar todo o conteúdo acima e salvar no seu arquivo com a extensão `.md`)*.
