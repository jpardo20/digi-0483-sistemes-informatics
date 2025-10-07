# Activitat 1: Desplega un servei web amb Docker

## Objectius
- Entendre la diferència entre imatge i contenidor.  
- Executar un servei web real amb Docker.  
- Practicar la gestió de ports i volums.  
- Aprendre a utilitzar `docker-compose` per automatitzar serveis.  

---

## Recordatori de conceptes
- Una **imatge** és una plantilla immutable (p. ex. `nginx:latest`).  
- Un **contenidor** és una instància viva d’aquesta imatge.  
- Els **volums** permeten guardar dades persistents.  
- Els **ports** permeten accedir al servei des de fora del contenidor.  

Esquema de connexió:
```
Host (Ubuntu) ──> Docker Engine ──> Contenidor (nginx) ──> Port 8080 ──> Navegador (http://192.168.56.101:8080)
```

---

## Part 1 – Llançament d’un servidor web

1. Comprova que Docker funciona:
   ```bash
   docker container list
   ```

2. Descarrega la imatge de **nginx**:
   ```bash
   docker pull nginx
   ```

3. Executa-la exposant el **port `8080`**:
   ```bash
   docker run -d -p 8080:80 --name web1 nginx
   ```

4. Comprova el funcionament:  
   - Obre el navegador del teu equip **(no dins la MV)** i escriu:  
      `http://192.168.56.101:8080`  
   - Hauries de veure la pàgina per defecte de Nginx.

5. Mostra els contenidors actius:
   ```bash
   docker container list
   ```

6. Atura i elimina el contenidor:
   ```bash
   docker stop web1
   docker rm web1
   ```

---

## 🗂️ Part 2 – Afegir contingut personalitzat

1. Crea un directori de treball:
   ```bash
   mkdir -p ~/projectes/docker-nginx/html
   cd ~/projectes/docker-nginx
   ```

2. Crea una pàgina HTML pròpia:
   ```bash
   echo "<h1>Hola, sóc el servidor de <em>$(whoami)</em></h1>" > html/index.html
   ```

3. Executa Nginx amb un volum:
   ```bash
   docker run -d -p 8080:80    -v ~/projectes/docker-nginx/html:/usr/share/nginx/html    --name web2 nginx
   ```

4. Comprova al navegador:  
   👉 `http://192.168.56.101:8080`  
   Ara hauries de veure la teva pròpia pàgina.

---

## ⚙️ Part 3 – Introducció a docker-compose

1. Crea el fitxer `docker-compose.yml` dins `~/projectes/docker-nginx`:
   ```yaml
   version: "3.8"
   services:
     web:
       image: nginx
       ports:
         - "8080:80"
       volumes:
         - ./html:/usr/share/nginx/html
   ```

2. Llença el servei:
   ```bash
   docker compose up -d
   ```

3. Comprova el funcionament:
   ```bash
   docker ps
   ```

4. Obre el navegador:  
   👉 `http://192.168.56.101:8080`  

5. Atura i elimina els serveis:
   ```bash
   docker compose down
   ```

---

## 💡 Part 4 – Mini repte final

**Repte:**  
Edita el teu `index.html` per incloure:
- Un títol amb el teu nom i cognom.  
- Una imatge (URL o local).  
- Una breu descripció del teu “servei web personal”.

Quan funcioni, fes:
```bash
docker compose down
tar czf projecte_web.tar.gz ~/projectes/docker-nginx
```

Prepara l’arxiu per entregar-lo al Classroom o al teu repositori GitHub.

---

## 🧾 Avaluació

| Criteri | Evidència | Pes |
|:--|:--|:--:|
| Execució correcta del contenidor `nginx` | Comprovació al navegador | 25% |
| Ús correcte de volums i ports | Comandes correctes | 25% |
| Configuració amb `docker-compose` funcional | Fitxer `docker-compose.yml` vàlid | 25% |
| Pàgina web personalitzada | Contingut visible i original | 25% |

---

**Mòdul:** 0483 – Sistemes informàtics  
**Resultat d’aprenentatge:** RA4 – Administra sistemes operatius multiusuari  
**Criteris d’avaluació:** CA4.d, CA4.e, CA4.f  
