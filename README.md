# 🖥️ Homelab-infra
**Lab personnel monté de zéro sous VirtualBox — Active Directory, GPO, RDP, SSH**

---

## 🎯 Objectif

Construire une infrastructure réseau complète simulant un environnement d'entreprise, avec un contrôleur de domaine Windows Server, des postes clients Windows et Linux, et des connexions à distance sécurisées.

---

## 🏗️ Architecture du lab

```
                    ┌─────────────────────────────┐
                    │      labreseau (interne)     │
                    │       192.168.1.0/24         │
                    └──────────────┬───────────────┘
                                   │
       ┌─────────────┬─────────────┼─────────────┐
       │             │             │             │
 ┌─────▼────┐  ┌─────▼────┐  ┌────▼─────┐  ┌───▼──────┐
 │ WinSRV   │  │WinClient │  │SRV-LINUX │  │CLIENT-   │
 │          │  │          │  │          │  │LINUX     │
 │ .1.10    │  │ .1.20    │  │ .1.30    │  │ .1.40    │
 └──────────┘  └──────────┘  └──────────┘  └──────────┘
```

---

## 🖥️ Machines virtuelles

| VM | OS | IP | Rôle |
|----|----|----|------|
| WinSRV | Windows Server 2022 Standard | 192.168.1.10 | Contrôleur de domaine (AD DS + DNS) |
| WinClient | Windows 10 Pro | 192.168.1.20 | Poste client joint au domaine |
| SRV-LINUX | Ubuntu Server 24.04 LTS | 192.168.1.30 | Serveur SSH Linux |
| CLIENT-LINUX | Ubuntu Desktop 24.04 LTS | 192.168.1.40 | Poste client Linux |

---

## ⚙️ Ce qui a été configuré

### 🪟 Windows Server

- Installation et configuration du rôle AD DS (Active Directory Domain Services)
- Création du domaine `lab.local`
- Configuration DNS automatique avec AD
- Attribution d'une IP fixe (192.168.1.10)
- Création d'utilisateurs AD (ex : adupont@lab.local)
- Déploiement d'une GPO (blocage du Panneau de configuration pour les utilisateurs du domaine)
- Activation et test du RDP (Bureau à distance) vers WinClient

![Active Directory](screenshots/ad-users.png)
![GPO](screenshots/gpo.png)

### 🪟 Windows 10 Pro (Client)

- Jonction au domaine `lab.local`
- Connexion avec un compte de domaine (LAB\adupont)
- Application des GPO via `gpupdate /force`
- Autorisation du RDP pour les utilisateurs du domaine

![Jonction domaine](screenshots/domain-join.png)
![RDP](screenshots/rdp.png)

### 🐧 Ubuntu Server 24.04 (SRV-LINUX)

- Configuration IP fixe via netplan (192.168.1.30)
- Installation et activation du service SSH (OpenSSH)
- Connexion SSH depuis WinSRV (Windows → Linux)
- Connexion SSH depuis CLIENT-LINUX (Linux → Linux)

### 🐧 Ubuntu Desktop 24.04 (CLIENT-LINUX)

- Configuration IP fixe via netplan (192.168.1.40)
- Tests de connectivité (ping, ssh)
- Connexion SSH vers SRV-LINUX

![SSH Linux](screenshots/ssh-linux.png)

---

## 🔗 Connexions à distance testées

| Source | Destination | Protocole | Résultat |
|--------|-------------|-----------|----------|
| WinSRV | WinClient | RDP | ✅ |
| WinSRV | SRV-LINUX | SSH | ✅ |
| CLIENT-LINUX | SRV-LINUX | SSH | ✅ |

![Connexions](screenshots/connexions.png)

---

## 🛠️ Stack technique

| Composant | Détail |
|-----------|--------|
| Hyperviseur | Oracle VirtualBox |
| Réseau | Réseau interne VirtualBox (labreseau) |
| OS Serveur | Windows Server Standard Evaluation / Ubuntu Server 24.04 LTS |
| OS Client | Windows 10 Pro / Ubuntu Desktop 24.04 LTS |
| Services | AD DS, DNS, GPO, RDP, SSH, netplan |
| Scripting | PowerShell, Bash |

---

## 👩‍💻 Auteure

**Mina OUAAZIZ** — Technicienne Supérieure Systèmes et Réseaux  
Passionnée par la cybersécurité défensive, l'administration système et le support IT.

