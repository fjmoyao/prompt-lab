# 🏷️ Anatomía del Prompt

Herramienta interactiva para que estudiantes identifiquen y etiqueten las partes de un prompt de inteligencia artificial. Diseñada para clases presenciales con hasta 40 estudiantes simultáneos.

---

## ¿Qué es esto?

Un par de archivos HTML que no requieren instalación ni servidor backend:

- **`estudiante.html`** — cada estudiante lo abre en su celular o PC y realiza 5 ejercicios de tagging
- **`profesor.html`** — el profesor lo proyecta en pantalla grande y ve los resultados en tiempo real

Los datos se sincronizan entre estudiantes y tablero usando `localStorage` + `BroadcastChannel`, lo que significa que **todos deben estar en el mismo dominio** (mismo hosting).

---

## Estructura del repositorio

```
prompt-lab/
├── estudiante.html     # App del estudiante
├── profesor.html       # Tablero del profesor
└── README.md           # Este archivo
```

---

## 🚀 Links en vivo

El proyecto ya está publicado en GitHub Pages:

| Archivo | URL |
|---|---|
| 👩‍🎓 Estudiantes | https://fjmoyao.github.io/prompt-lab/estudiante.html |
| 👨‍🏫 Profesor | https://fjmoyao.github.io/prompt-lab/profesor.html |
| 📁 Repositorio | https://github.com/fjmoyao/prompt-lab |

---

## Cómo hacer el deploy (referencia para futuros cambios)

### Paso 1 — Crear el repositorio

1. Ve a [github.com](https://github.com) e inicia sesión
2. Haz clic en **New repository**
3. Nombre del repositorio: `prompt-lab`
4. Márcalo como **Public** (requerido para GitHub Pages gratis)
5. Haz clic en **Create repository**

### Paso 2 — Subir los archivos

**Opción A — Desde el navegador (más fácil):**
1. Dentro del repositorio recién creado, haz clic en **Add file → Upload files**
2. Arrastra los tres archivos: `estudiante.html`, `profesor.html`, `README.md`
3. Haz clic en **Commit changes**

**Opción B — Con Git (si tienes Git instalado):**
```bash
git clone https://github.com/fjmoyao/prompt-lab.git
cd prompt-lab
# copia los tres archivos aquí
git add .
git commit -m "primera versión"
git push origin main
```

### Paso 3 — Activar GitHub Pages

1. Ve a **Settings** (pestaña del repositorio)
2. En el menú izquierdo, clic en **Pages**
3. En **Source**, selecciona **Deploy from a branch**
4. En **Branch**, selecciona `main` y carpeta `/ (root)`
5. Haz clic en **Save**
6. Espera 1–2 minutos y tu sitio estará en:

```
https://fjmoyao.github.io/prompt-lab/
```

### Paso 4 — Obtener los links finales

| Archivo | URL |
|---|---|
| Estudiantes | `https://fjmoyao.github.io/prompt-lab/estudiante.html` |
| Profesor | `https://fjmoyao.github.io/prompt-lab/profesor.html` |

---

## Cómo usarlo en clase

### Antes de la sesión
- [ ] Verifica que el deploy esté activo abriendo el link del estudiante en tu celular
- [ ] Abre `profesor.html` en el computador que vas a proyectar
- [ ] Ten el link de `estudiante.html` listo para compartir (QR, chat, o escrito en el tablero)

### Durante la sesión

1. **Comparte el link** `estudiante.html` con los estudiantes (WhatsApp, Slack, o proyecta el QR)
2. **Pide que escriban su nombre** completo en la pantalla de bienvenida
3. **Proyecta** `profesor.html` mientras ellos trabajan — se actualiza cada 5 segundos automáticamente
4. **Observa en tiempo real** qué etiqueta les cuesta más (la gráfica de precisión por tipo lo muestra claramente)
5. Al terminar, usa los resultados como punto de partida para la discusión en clase

### Después del ejercicio
- La columna **"Por prompt"** en el tablero muestra qué prompts tuvieron más errores → úsalos para el repaso
- La sección **"Necesitan apoyo"** (bajo 60%) te indica quiénes requieren atención individualizada
- Para reiniciar para una nueva sesión: botón **"Reiniciar sesión"** en el tablero del profesor

---

## Los 5 ejercicios incluidos

Todos los prompts usan el mismo esquema de etiquetas:

| Etiqueta | Color | Qué identifica |
|---|---|---|
| 🔵 **Acción** | Azul | El verbo principal: qué debe hacer la IA |
| 🟢 **Contexto** | Verde | Rol, audiencia y situación |
| 🟡 **Modificador** | Ámbar | Formato, tono y extensión |

| # | Tema | Prompt resumido |
|---|---|---|
| 1 | Cocina | Chef mediterráneo explica cómo preparar paella |
| 2 | Salud | Médico explica la diabetes tipo 2 a un paciente recién diagnosticado |
| 3 | Trabajo | Consultor de RRHH redacta correo para pedir aumento |
| 4 | Finanzas | Asesor financiero enseña a ahorrar con salario básico |
| 5 | Tecnología | Experto en ciberseguridad protege cuentas de adulto mayor |

---

## Cómo agregar o editar ejercicios

Abre `estudiante.html` y busca el array `EXERCISES` cerca del inicio del bloque `<script>`. Cada ejercicio tiene esta estructura:

```javascript
{
  id: 6,                          // número único
  prompt: "Texto completo del prompt...",
  tokens: [
    { text: "Eres un experto en X.", tag: "contexto" },
    { text: " Explícame cómo hacer Y.", tag: "accion" },
    { text: " Tengo Z años y no sé nada.", tag: "contexto" },
    { text: " Dámelo en lista.", tag: "modificador" },
    { text: " Tono formal.", tag: "modificador" }
  ],
  hint: "Pista pedagógica que ve el estudiante durante el ejercicio."
}
```

> **Importante:** el `text` de cada token debe coincidir exactamente con una parte del `prompt`. Los espacios al inicio de cada fragmento (excepto el primero) son necesarios para la separación visual.

Si agregas ejercicios, actualiza también el texto `"Prompt X de 5"` cambiando el `5` por el nuevo total en la función `loadExercise`.

---

## Compatibilidad

| Dispositivo | Soporte |
|---|---|
| Chrome (desktop) | ✅ Completo |
| Firefox (desktop) | ✅ Completo |
| Safari (iOS) | ✅ Completo |
| Chrome (Android) | ✅ Completo |
| Samsung Internet | ✅ Completo |

> El tablero del profesor funciona mejor en pantalla grande (laptop o monitor).

---

## Limitaciones técnicas conocidas

- **Los datos son locales al navegador.** Si el profesor abre el tablero en un dispositivo diferente al que usan los estudiantes, no verá los resultados. Todos deben estar en el mismo dominio y el mismo origen de `localStorage`. En la práctica: estudiantes en sus dispositivos → datos guardados en el localStorage de cada uno → sincronizados al tablero vía BroadcastChannel → **funciona solo si todos están en la misma sesión de navegador del mismo origen (mismo GitHub Pages URL).**

- **Sin persistencia entre sesiones.** Al cerrar el navegador o hacer clic en "Reiniciar sesión", los datos se borran. Si quieres guardar resultados, exporta manualmente la tabla antes de cerrar (captura de pantalla o copia).

- **BroadcastChannel requiere mismo origen.** Funciona perfectamente cuando todo está hosteado en el mismo GitHub Pages. No funciona si abres los archivos directamente desde el escritorio (`file://`).

---

## Contexto pedagógico

Esta herramienta fue diseñada para enseñar la **anatomía de un prompt** de IA usando tres bloques:

```
ACCIÓN + CONTEXTO + MODIFICADORES
```

| Bloque | Pregunta clave | Elementos |
|---|---|---|
| Acción | ¿Qué debe hacer la IA? | Tarea |
| Contexto | ¿Desde dónde y para quién? | Rol + Audiencia + Situación |
| Modificadores | ¿Cómo debe ser la respuesta? | Formato + Tono + Extensión |

Para más contexto sobre la metodología, consulta los materiales del curso.

---

## Licencia

Libre para uso educativo. Puedes modificar, adaptar y redistribuir con atribución.

---

*Creado para uso en clase · 2026*
