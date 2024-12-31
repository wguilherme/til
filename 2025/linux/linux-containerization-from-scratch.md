# TIL: Criando ambientes isolados no Linux do zero

Hoje aprendi os fundamentos de containerização no Linux, explorando as tecnologias base que permitem criar ambientes isolados.

## Namespaces com unshare
Namespaces isolam recursos do sistema. Criar um namespace básico:

# Criar namespace isolando PID, rede, mount, IPC e hostname
unshare --pid --net --mount --ipc --uts --fork /bin/bash

# Ver namespaces de um processo
ls -l /proc/[PID]/ns/

## Control Groups (cgroups)
Limitam recursos do sistema:

# Criar cgroup para memória
mkdir /sys/fs/cgroup/memory/container1
echo "100M" > /sys/fs/cgroup/memory/container1/memory.limit_in_bytes

# Criar cgroup para CPU
mkdir /sys/fs/cgroup/cpu/container1
echo "512" > /sys/fs/cgroup/cpu/container1/cpu.shares

# Adicionar processo ao cgroup
echo $PID > /sys/fs/cgroup/memory/container1/tasks

## Chroot
Isola sistema de arquivos:

# Instalar debootstrap
apt-get install debootstrap

# Criar sistema de arquivos base
debootstrap --arch amd64 bullseye /container http://deb.debian.org/debian/

# Fazer chroot
mount --bind /proc /container/proc
mount --bind /sys /container/sys
mount --bind /dev /container/dev
chroot /container /bin/bash

## Monitoramento de Processos

# Listar processos
ps aux

# Ver informações de um processo específico
cat /proc/[PID]/status
cat /proc/[PID]/cmdline

# Verificar dependências de binários
ldd /bin/bash

## Evolução dos Containers
- LXC (Linux Containers): primeira implementação de containers no Linux
- LXD: hypervisor para LXC, facilitando gerenciamento
- Docker: inicialmente usava LXC, depois desenvolveu containerD
- containerD: runtime de container desenvolvido pelo Docker, agora projeto independente

## Estrutura do /proc
/proc/[PID]/
  ├── cmdline      # Comando que iniciou o processo
  ├── environ      # Variáveis de ambiente
  ├── exe          # Link para o executável
  ├── fd/          # File descriptors
  ├── limits       # Limites de recursos
  ├── mounts       # Pontos de montagem
  ├── status       # Status do processo
  └── task/        # Threads do processo

## LXC vs Docker
LXC:
- Foco em virtualização no nível do sistema operacional
- Mais próximo de VMs tradicionais
- Geralmente sistema completo

Docker:
- Foco em aplicações
- Containers mais leves
- Um processo principal por container

Este aprendizado mostra como tecnologias como Docker e LXC funcionam internamente, usando recursos nativos do kernel Linux para criar isolamento.