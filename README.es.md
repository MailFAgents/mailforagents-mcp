<p align="center">
  <img src="assets/logo.svg" alt="mailforagents" height="48" />
</p>

<h3 align="center">Buzones de correo para agentes de IA, por MCP</h3>

<p align="center">
  <a href="https://mailforagents.io/es/">Sitio</a> ·
  <a href="https://app.mailforagents.io">App</a> ·
  <a href="https://mailforagents.io/es/blog/conectar-buzon-a-chatgpt-o-claude/">Guía de conexión</a> ·
  <a href="README.md">English</a>
</p>

---

**mailforagents** le da a cada agente de IA su propio buzón de correo. Este repositorio documenta el **servidor MCP remoto**: una sola URL a la que Claude, ChatGPT, Cursor, Claude Code y cualquier cliente MCP se conectan para que el agente lea, busque, responda, envíe y haga envíos masivos, y lea adjuntos, desde el buzón que tú elijas.

```
https://api.mailforagents.io/mcp
```

- **Transporte:** Streamable HTTP (compatible con SSE)
- **Autenticación:** OAuth 2.1 con registro dinámico de clientes y PKCE. Inicias sesión y eliges **qué buzón** puede usar el cliente; los demás no los ve.
- **Alternativa:** la URL de cada buzón con un token, para archivos de configuración y CI.
- **MCP Registry oficial:** [`io.mailforagents/mailforagents`](https://registry.modelcontextprotocol.io/v0.1/servers?search=io.mailforagents)

> Este repositorio tiene la documentación, el manifiesto del registro (`server.json`) y configuraciones de ejemplo. El servidor está alojado: no hay nada que instalar.

## Para empezar

1. Crea un buzón gratis en [mailforagents.io](https://mailforagents.io/es/), por ejemplo `soporte@agents.mailforagents.io`.
2. Agrega la URL del servidor a tu cliente (abajo).
3. Inicia sesión cuando te lo pida y elige el buzón.
4. Pídele a tu agente: *«Revisa mi buzón: ¿cuántos correos sin leer tengo y qué llegó hoy?»*

### Claude (web, escritorio, móvil)

**Personalizar → Conectores → Agregar → Agregar conector personalizado**, pega `https://api.mailforagents.io/mcp`, pulsa **Conectar** y elige tu buzón.

### Claude Code

```bash
claude mcp add --transport http mailforagents https://api.mailforagents.io/mcp --scope user
```

Después escribe `/mcp` en una sesión para iniciar sesión.

### ChatGPT

**Plugins → + → Add custom MCP server** (flujo anterior: Configuración → Apps → Crear, con el modo desarrollador). Pega `https://api.mailforagents.io/mcp`, elige **OAuth**, inicia sesión y elige tu buzón. El MCP completo, con envío, está en Business y Enterprise/Edu.

### Cursor, VS Code y otros clientes

```json
{
  "mcpServers": {
    "mailforagents": { "url": "https://api.mailforagents.io/mcp" }
  }
}
```

Más ejemplos en [`examples/`](examples/).

### Con token en vez de OAuth

Crea un token en la pestaña **Conexión MCP** del buzón (solo lectura, lectura y envío, o completo; con vencimiento) y usa la URL del buzón:

```
URL:    https://api.mailforagents.io/mcp/<id del buzón>
Header: Authorization: Bearer mfa_mcp_...
```

## Herramientas

`get_mailbox`, `list_messages`, `search_messages`, `get_thread`, `send_message`, `reply`, `forward`, `create_draft`, `update_draft`, `list_attachments`, `get_attachment` (PDF, Word, Excel y PowerPoint como texto), `upload_file`, `move_message`, `add_label`, `massive_send` (una llamada, muchos destinatarios, con variables), `get_massive_send`, `retry_message` y `update_mailbox` (cambia el nombre visible y la firma). El detalle está en el [README en inglés](README.md#tools).

## Seguridad

- **Un buzón por agente**, no tu correo personal. Cada autorización y cada token sirven para un solo buzón.
- **Aprobación humana:** si la activas, los correos y los envíos masivos esperan a que alguien los apruebe en el panel.
- **Listas de permitidos y bloqueados** y límites diarios de envío.
- **Tokens de solo lectura** para resúmenes y clasificación.
- El contenido de los correos es de terceros: el servidor le indica al modelo que no siga instrucciones que vengan dentro de un correo.

## Planes

Gratis (1 buzón, 3.000 correos al mes), Starter (USD 12) y Pro (USD 120). Precios vigentes en [mailforagents.io](https://mailforagents.io/es/).
