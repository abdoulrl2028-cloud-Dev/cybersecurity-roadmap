# Comandos Linux Essenciais

## Navegação e Estrutura de Arquivos

### ls
Lista arquivos e diretórios.
```bash
ls                      # Listagem simples
ls -l                   # Listagem longa (permissions)
ls -la                  # Incluindo arquivos ocultos
ls -lS                  # Ordenado por tamanho
ls -lt                  # Ordenado por data
```

### pwd
Exibe o diretório atual.
```bash
pwd                     # Print Working Directory
```

### cd
Navega entre diretórios.
```bash
cd /home/user          # Caminho absoluto
cd ..                  # Diretório pai
cd -                   # Último diretório
cd ~                   # Home do usuário
```

### mkdir
Cria diretórios.
```bash
mkdir pasta
mkdir -p pasta/sub/estrutura   # Cria recursivamente
```

---

## Manipulação de Arquivos

### cp
Copia arquivos e diretórios.
```bash
cp arquivo1 arquivo2
cp -r pasta1 pasta2    # Copia diretório recursivamente
cp arquivo /novo/local/
```

### mv
Move ou renomeia arquivos.
```bash
mv arquivo novo_nome
mv arquivo /novo/local/
mv *.txt /destino/     # Move múltiplos arquivos
```

### rm
Remove arquivos e diretórios.
```bash
rm arquivo
rm -r pasta            # Remove diretório recursivamente
rm -f arquivo          # Force (sem confirmação)
rm *.txt               # Remove padrão
```

### touch
Cria arquivo vazio ou atualiza timestamp.
```bash
touch novo_arquivo
touch arquivo          # Atualiza hora de modificação
```

### cat
Exibe conteúdo de arquivo.
```bash
cat arquivo.txt
cat arquivo1 arquivo2   # Concatena múltiplos
```

### less / more
Visualiza arquivo com paginação.
```bash
less arquivo.txt       # Melhor (q para sair, space para próxima)
more arquivo.txt
```

### tail
Mostra últimas linhas de arquivo.
```bash
tail -n 20 arquivo.txt
tail -f arquivo.log    # Monitorar em tempo real
```

### head
Mostra primeiras linhas.
```bash
head -n 10 arquivo.txt
```

---

## Permissões

### chmod
Muda permissões de arquivo.
```bash
chmod 755 script.sh         # rwxr-xr-x
chmod 644 arquivo.txt       # rw-r--r--
chmod +x script.sh          # Adiciona execução
chmod -r+w pasta/          # Remove leitura, adiciona escrita
```

### chown
Muda proprietário do arquivo.
```bash
chown user arquivo
chown user:group arquivo
chown -R user:group pasta/  # Recursivamente
```

### chgrp
Muda grupo do arquivo.
```bash
chgrp group arquivo
chgrp -R group pasta/
```

---

## Busca e Filtragem

### find
Busca arquivos por critério.
```bash
find . -name "*.txt"
find /home -type f -name "arquivo*"
find . -size +100M
find . -mtime -7        # Modificado últimos 7 dias
find . -name "*.log" -delete
```

### grep
Busca padrão em arquivos.
```bash
grep "palavra" arquivo.txt
grep -r "palavra" .          # Recursivamente
grep -i "PALAVRA" arquivo    # Case insensitive
grep -v "palavra" arquivo    # Inverso (não contém)
grep -n "palavra" arquivo    # Com número de linha
```

### grep extended
```bash
grep -E "[0-9]{3}" arquivo   # Expressão regular
egrep "pattern" arquivo      # Alias para grep -E
```

---

## Processamento de Texto

### sed
Stream editor (busca e substitui).
```bash
sed 's/antigo/novo/' arquivo.txt
sed 's/antigo/novo/g' arquivo.txt    # Global
sed -i 's/antigo/novo/g' arquivo     # In place
```

### awk
Processamento de linhas e campos.
```bash
awk '{print $1}' arquivo              # Campo 1
awk -F',' '{print $2}' arquivo.csv    # Delimiter ','
awk '{sum+=$1} END {print sum}' arquivo
```

### cut
Extrai colunas.
```bash
cut -d',' -f2 arquivo.csv    # Campo 2, delimiter ','
cut -c1-10 arquivo          # Caracteres 1-10
```

---

## Processos

### ps
Lista processos em execução.
```bash
ps aux                  # Todos os processos
ps aux | grep nome
ps -ef
ps -u username
```

### top
Monitor de processos interativo.
```bash
top                     # Pressione q para sair
top -u username
```

### htop
Alternativa melhorada (mais intuitiva).
```bash
htop
```

### kill
Encerra processos.
```bash
kill PID
kill -9 PID             # Force kill
killall processo        # Por nome
```

### bg / fg
Controla jobs em background/foreground.
```bash
comando &              # Executa em background
bg                     # Resume job em background
fg                     # Traz para foreground
jobs                   # Lista jobs
```

---

## Permissões do Sistema

### sudo
Executa como superusuário.
```bash
sudo comando
sudo -l                 # Lista permissões
sudo su                 # Vira root
```

### su
Troca de usuário.
```bash
su username
su -                    # Vira root
```

---

## Gerenciamento de Pacotes

### apt (Debian/Ubuntu)
```bash
sudo apt update         # Atualiza lista
sudo apt upgrade        # Atualiza pacotes
sudo apt install pacote
sudo apt remove pacote
sudo apt search palavra
```

### yum (RedHat/CentOS)
```bash
sudo yum install pacote
sudo yum remove pacote
```

---

## Serviços do Sistema

### systemctl
Gerencia serviços.
```bash
systemctl start apache2
systemctl stop apache2
systemctl restart apache2
systemctl status apache2
systemctl enable apache2    # Auto-inicia no boot
systemctl disable apache2
```

---

## Compactação

### tar
Cria arquivos tarball.
```bash
tar -cvf arquivo.tar pasta/
tar -xvf arquivo.tar
tar -czvf arquivo.tar.gz pasta/    # Com gzip
tar -xzvf arquivo.tar.gz
```

### gzip / gunzip
```bash
gzip arquivo
gunzip arquivo.gz
```

### zip / unzip
```bash
zip -r arquivo.zip pasta/
unzip arquivo.zip
```

---

## Redirecionamento

### Operadores
```bash
comando > arquivo       # Salva output em arquivo
comando >> arquivo      # Append
comando < arquivo       # Input do arquivo
comando 2> erro.log     # Redireciona erros
comando &> tudo.log     # Output e erros
comando | comando2      # Pipe (output → input)
```

---

## Informações do Sistema

### uname
Info do sistema.
```bash
uname -a                # Todas as informações
uname -r                # Versão do kernel
```

### whoami
Usuário atual.
```bash
whoami
```

### df / du
Espaço em disco.
```bash
df -h                   # Espaço de partições
du -sh pasta/           # Tamanho da pasta
du -h --max-depth=1     # Um nível de profundidade
```

### free
Memória disponível.
```bash
free -h
free -m
```

---

## Variáveis de Ambiente

```bash
echo $PATH              # Variável PATH
echo $HOME              # Diretório home
export VAR="valor"      # Define variável
```

---

## Dicas Importantes

1. **Help**: `man comando` ou `comando --help`
2. **Wildcard**: `*` qualquer, `?` um caractere
3. **Pipes**: Encadeie comandos com `|`
4. **Aliases**: `alias ll='ls -la'`
