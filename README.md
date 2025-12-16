# Prática 5: Configuração do SystemD e Controle de Versão com Git

## Autor
  Guilherme Pedro de Oliveira Souza 
  N°USP11801178

## Resumo da Prática

A prática teve como objetivo configurar um serviço de inicialização personalizada para sistemas embarcados com Linux, usando o SystemD, e controlar versões dos arquivos de código utilizando Git e GitHub. Foi configurado um serviço SystemD que inicializa automaticamente um script de controle de LED em uma Raspberry Pi durante o boot do sistema. Além disso, foram demonstrados processos de versionamento com Git e uso do GitHub para organizar e compartilhar o código.

## Estrutura do Repositório

O repositório contém os seguintes arquivos:

1. `blinkgpiozero.py` — Código Python que aciona um LED na Raspberry Pi usando a biblioteca `gpiozero`.
2. `blink2.service` — Arquivo de configuração do SystemD para gerenciar o serviço de inicialização do LED.
3. `Roteiro_pratica-5_eletrica.pdf` — Documento com o roteiro da prática, explicando cada passo do processo.
4. `gitlog.txt` — Histórico de commits realizados no repositório local, gerado com `git log`.
5. `README.md` — Este arquivo de documentação.

## Passos Realizados

### 1. Configuração do SystemD para Inicialização do LED
- Criado um script Bash para controlar o LED conectado à GPIO da Raspberry Pi.
- Configurado o arquivo de serviço do SystemD (`blink2.service`) para inicializar automaticamente o script de LED durante o boot.
- O serviço foi testado com `systemctl start blink` e `systemctl enable blink` para garantir sua execução durante a inicialização.

### 2. Controle de Versão com Git
- Inicializado um repositório Git na Raspberry Pi com `git init` / `git clone`.
- Utilizados `git add`, `git commit`, `git push` e `git pull` para versionar e compartilhar o código.
- O log de commits foi gerado e armazenado em `gitlog.txt` para rastrear as alterações.

### 3. Uso do GitHub
- Criado um repositório no GitHub para armazenar o código e a documentação da prática.
- Enviado as atualizações para o GitHub usando `git push` com autenticação por token (conforme tutorial do GitHub).

## Observação sobre Autor do Commit e Push

Ao realizar `git push`, o autor do commit registrado depende da configuração local de usuário (`git config user.name` / `user.email`). No caso descrito, os commits locais foram feitos com o usuário `viniciuscaffeu` que já estava inserido na raspberry, enquanto o push foi autenticado com o token da minha conta. Isso faz com que o autor do commit apareça diferente do usuário que autenticou o push.

## Como Reproduzir

### 1. Configuração do Git e GitHub
```bash
git config --global user.name "seu_nome"
git config --global user.email "seu_email"
# criar repositório no GitHub e clonar:
git clone https://github.com/seu_usuario/seu_repositorio.git
```

### 2. Criação do Script de Blink (exemplo)
Crie um arquivo `blink.sh` com permissão de execução:

```bash
#!/bin/bash
echo 18 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio18/direction

while true; do
  echo 1 > /sys/class/gpio/gpio18/value
  sleep 0.2
  echo 0 > /sys/class/gpio/gpio18/value
  sleep 0.2
done
```

### 3. Arquivo do Service do SystemD
Crie `blink2.service` com o conteúdo abaixo (ajuste `ExecStart` e `User` conforme seu sistema):

```ini
[Unit]
Description=Blink LED
After=multi-user.target

[Service]
ExecStart=/home/sel/blink.sh
User=sel
Restart=always

[Install]
WantedBy=multi-user.target
```

### 4. Copiar o Serviço para o SystemD e Habilitar
```bash
sudo cp blink2.service /lib/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable blink2.service
sudo systemctl start blink2.service
```

### 5. Enviar para o GitHub
```bash
git add README.md
git commit -m "Atualização do readme da prática 5"
git push origin main
```

## Observações Finais

Esta prática abordou a configuração do SystemD para gerenciar serviços de inicialização em sistemas embarcados, o uso do Git para controle de versão e a integração com o GitHub. O projeto foi testado em uma Raspberry Pi e a funcionalidade foi verificada após o boot do sistema.

---
Autor: Gabriel Pedro Souza (gpedrosouza)
