# Xestor de xornadas de Patios Dinámicos para Docentes

[![Licenza: CC BY 4.0](https://licensebuttons.net/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/deed.gl)

Aplicación web para a xestión do programa **Patios Dinámicos** dun centro educativo: planificación de xornadas, catálogo de xogos, historial, avaliación por zonas e xeración de informes (DAFO/valoración).

Desenvolvida orixinalmente para o **CEIP Plurilingüe Santo Estevo de Parga** (Galicia).

---

## Que fai

- **Planificación de xornadas**: organización de zonas, xogos, espazos, docentes responsables e alumnado axudante por sesión.
- **Catálogo de xogos**: listaxe de xogos por tipo, con materiais e iconas, ampliable polo profesorado.
- **Historial**: rexistro de todas as xornadas, agrupadas por trimestre, con xeración de PDF.
- **Avaliación por zonas**: cada docente avalía os xogos das zonas onde participou.
- **DAFO / Valoración**: resumo automático do curso e rexistro de revisións (función da administración).
- **Configuración**: claustro, espazos, tipos de xogo, alumnado axudante (función da administración).
- **Sistema de presenza**: aviso en tempo real cando varias persoas están a editar á vez.

## Como está construída

- **Un único ficheiro HTML** (`index.html`) que inclúe a estrutura, os estilos e a lóxica. Non require compilación nin instalación de dependencias.
- **Firebase Realtime Database** (SDK compat v10) para gardar e sincronizar os datos entre dispositivos.
- **GitHub Pages** para a publicación.

## Modelo de acceso

A aplicación ábrese en **modo lectura**: calquera persoa pode consultar todo sen iniciar sesión.

Para editar hai dous tipos de usuario:

- **Administración**: control total (configuración, DAFO, todas as xornadas).
- **Profesorado**: créase automaticamente a partir da listaxe do claustro. Cada docente pode crear e editar xogos, planificar xornadas (queda como coordinador/a), editar as xornadas que coordina e avaliar as zonas onde participou.

> **Aviso importante sobre a seguridade.** Este sistema de usuarios é unha axuda de **organización**, non un mecanismo de seguridade real. As contrasinais derívanse do nome (`patios_nome`) e son visibles no código. Non protexe datos fronte a usos malintencionados; está pensado para o traballo de boa fe dun claustro. **Non introduzas datos persoais identificables do alumnado**; usa iniciais.

---

## Instalación nun centro novo

Calquera centro pode implementar a súa propia copia. Necesítanse unha conta de GitHub e unha conta de Google (para Firebase), ambas gratuítas.

### 1. Copiar o proxecto

1. Crea un repositorio novo na túa conta de GitHub (ou fai unha copia deste).
2. Sube o ficheiro `index.html` e o ficheiro `LICENSE`.

### 2. Crear a base de datos en Firebase

1. Entra en [https://console.firebase.google.com](https://console.firebase.google.com) e crea un proxecto novo.
2. No menú **Realtime Database**, crea unha base de datos (rexión recomendada: `europe-west1`).
3. En **Configuración do proxecto → As túas aplicacións**, rexistra unha aplicación web e copia o obxecto de configuración (`firebaseConfig`).
4. No ficheiro `index.html`, busca o bloque `firebase.initializeApp({ ... })` e substitúe os valores polos do teu proxecto:

   ```js
   const _fbApp = firebase.initializeApp({
     apiKey: "A_TÚA_CLAVE",
     authDomain: "o-teu-proxecto.firebaseapp.com",
     databaseURL: "https://o-teu-proxecto-default-rtdb.europe-west1.firebasedatabase.app",
     projectId: "o-teu-proxecto",
     storageBucket: "o-teu-proxecto.firebasestorage.app",
     messagingSenderId: "...",
     appId: "..."
   });
   ```

### 3. Regras da base de datos

Como a aplicación non usa autenticación de Firebase, as regras deben permitir lectura e escritura públicas. En **Realtime Database → Regras**:

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> Isto implica que a base de datos é pública. Non gardes nela información sensible nin datos persoais identificables.

### 4. Publicar con GitHub Pages

1. No repositorio, vai a **Settings → Pages**.
2. En **Source**, escolle a rama (`main`) e a carpeta raíz (`/root`).
3. Garda. En poucos minutos a aplicación estará dispoñible no enderezo que indica GitHub.

### 5. Adaptar ao teu centro

Inicia sesión como administración (`admin` / `patiosparga`) e, na pestana **Configuración**, axusta o claustro, os espazos, os tipos de xogo e o alumnado axudante. Recoméndase cambiar a contrasinal de administración no código antes de usala de forma real (busca `ADMIN_PASS` no ficheiro).

---

## Desenvolvemento local

Para probar cambios no ordenador antes de publicar, abre o `index.html` directamente no navegador. Firebase non funciona co protocolo `file://`, así que durante as probas locais a aplicación mostra só os valores por defecto; a sincronización funciona unha vez publicada en GitHub Pages (protocolo `https://`).

---

## Implementación noutro centro

Os centros interesados en implementar este xestor poden solicitalo como **actividade de formación do PFPP** (Plan de Formación Permanente do Profesorado).

Contacto: **evamariacorredoira@edu.xunta.gal**

---

## Autoría e licenzas

- **Idea, deseño pedagóxico e planificación**: Eva C. Varela ([ORCID](https://orcid.org/0009-0008-4320-4886))
- **Programación**: Claude (Anthropic)

© 2026 Eva C. Varela

Este proxecto distribúese baixo **dúas licenzas**:

- O **concepto, o deseño pedagóxico e a planificación** do programa están baixo licenza **Creative Commons Recoñecemento 4.0 Internacional (CC BY 4.0)**: pódense copiar, adaptar e implementar noutros centros sempre que se manteña o recoñecemento da autoría de Eva C. Varela.
- O **código fonte** está baixo licenza **MIT**.

Consulta o ficheiro [`LICENSE`](LICENSE) para os textos completos.
