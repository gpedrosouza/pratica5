Prática 5: Configuração do SystemD e Controle de Versão com Git
Resumo da Prática

A prática teve como objetivo configurar um serviço de inicialização personalizada para sistemas embarcados com Linux, usando o SystemD, e controlar versões dos arquivos de código utilizando Git e GitHub. Foi realizada a configuração de um serviço SystemD que inicializa automaticamente um script de controle de LED em uma Raspberry Pi durante o boot do sistema. Além disso, o processo de versionamento com Git e o uso do repositório GitHub foi abordado para organizar e compartilhar o código.

Estrutura do Repositório

O repositório contém os seguintes arquivos:

blinkgpiozero.py: Código Python que aciona um LED na Raspberry Pi usando a biblioteca gpiozero.

blink2.service: Arquivo de configuração do SystemD para gerenciar o serviço de inicialização do LED.

Roteiro_pratica-5_eletrica.pdf: O documento com o roteiro da prática, explicando cada passo do processo.

gitlog.txt: Histórico de commits realizados no repositório local, gerado com o comando git log.

README.md: Este arquivo de documentação.

Passos Realizados

Configuração do SystemD para Inicialização do LED:

Criado um script Bash para controlar o LED conectado à GPIO da Raspberry Pi.

Configurado um arquivo de serviço do SystemD (blink2.service) para inicializar automaticamente o script de LED durante o processo de boot.

O serviço foi testado com os comandos systemctl start blink e systemctl enable blink para garantir sua execução durante a inicialização.

Controle de Versão com Git:

Inicializado um repositório Git na Raspberry Pi com os comandos git init e git clone.

Utilizado os comandos git add, git commit, git push, e git pull para versionar e compartilhar o código.

O log de commits foi gerado e armazenado em gitlog.txt para rastrear as alterações feitas no código.

Uso do GitHub:

Criado um repositório no GitHub para armazenar o código e a documentação da prática.

Realizado o envio das atualizações para o GitHub com o comando git push, utilizando um token de segurança para autenticação (conforme o tutorial do GitHub para geração de Token).

Explicação sobre o "Push" e o Autor do Commit

Observe que, ao realizar o git push, o autor do commit esteve diferente do dono do repositório no GitHub. Isso ocorreu porque o commit foi feito localmente com o usuário configurado na Raspberry Pi (viniciusouzacaffeu), enquanto o repositório remoto é de meu usuário. Mas como pode ser visto no gitlog, a autenticação  do push foi realizada com um token de segurança gerado pela minha conta do GitHub.

Como Reproduzir
1. Configuração do Git e GitHub

Execute git config --global user.name "seu_nome" e git config --global user.email "seu_email".

Crie um repositório no GitHub.

Clone o repositório com git clone https://github.com/seu_usuario/seu_repositorio.git.

2. Criação do Serviço SystemD

Crie o script blink.sh conforme o exemplo abaixo:

#!/bin/bash
echo 18 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio18/direction
while [ 1 ]
do
    echo 1 > /sys/class/gpio/gpio18/value
    sleep 0.2s
    echo 0 > /sys/class/gpio/gpio18/value
    sleep 0.2s
done

3. Arquivo do Service do SystemD

Crie um arquivo chamado blink2.service com o seguinte conteúdo:

[Unit]
Description=Blink LED
After=multi-user.target

[Service]
ExecStart=/home/sel/blink.sh
User=SEL

[Install]
WantedBy=multi-user.target

4. Copiar o Serviço para o Diretório do SystemD

Copie o arquivo blink2.service para o diretório /lib/systemd/system/:

sudo cp blink2.service /lib/systemd/system/


Habilite o serviço para iniciar no boot:

sudo systemctl enable blink

5. Enviar para o GitHub

Após criar e testar o serviço, faça o commit e envie os arquivos para o repositório no GitHub:

git add .
git commit -m "Adicionando arquivos do serviço Blink e Git"
git push

Observações Finais

Esta prática abordou a configuração do SystemD para gerenciar serviços de inicialização em sistemas embarcados, o uso do Git para controle de versão e a integração com o GitHub para armazenar e compartilhar o código. O projeto foi testado em uma Raspberry Pi e a funcionalidade foi verificada após o boot do sistema.
