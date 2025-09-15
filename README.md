# Módulos LLM para Odoo 15

Este repositorio contiene módulos para integrar Large Language Models (LLMs) con Odoo 15, adaptados desde la versión original para Odoo 16.

## Módulos Incluidos

### 1. LLM Base (`llm`)
- **Versión**: 15.0.1.4.0
- **Descripción**: Módulo base que proporciona la infraestructura fundamental para la integración con LLMs
- **Características**:
  - Gestión de proveedores de LLM (OpenAI, Anthropic, Ollama, etc.)
  - Sistema de mensajería optimizado para IA
  - Gestión de modelos y editores
  - Framework de seguridad y permisos

### 2. LLM Tool (`llm_tool`)
- **Versión**: 15.0.3.0.0
- **Descripción**: Framework de ejecución de herramientas y function calling
- **Características**:
  - Ejecución de funciones por parte de LLMs
  - Herramientas predefinidas para operaciones CRUD en Odoo
  - Sistema de consentimiento de usuario
  - Integración con el protocolo MCP

### 3. LLM MCP (`llm_mcp`)
- **Versión**: 15.0.1.0.0
- **Descripción**: Integración con el Model Context Protocol
- **Características**:
  - Conexión con servidores MCP externos
  - Descubrimiento automático de herramientas
  - Comunicación estándar I/O con procesos externos

## Cambios para Odoo 15

### Principales Adaptaciones

1. **Campos JSON → Text**: Los campos `Json` se han convertido a `Text` para compatibilidad con Odoo 15
2. **Widgets de Vista**: Actualizados los widgets `json_editor` y `json_inline` a `text`
3. **Manejo de JSON**: Implementado parsing manual de JSON en campos Text
4. **Migraciones**: Adaptadas las migraciones para la nueva estructura de datos

### Compatibilidad

- ✅ **Odoo 15.0**: Totalmente compatible
- ✅ **Python 3.8+**: Soportado
- ✅ **Funcionalidad**: Mantenida al 100%

## Instalación

1. **Clonar el repositorio**:
   ```bash
   git clone <repository-url>
   cd odoo-llm-15
   ```

2. **Copiar módulos a addons**:
   ```bash
   cp -r llm llm_tool llm_mcp /path/to/odoo/addons/
   ```

3. **Instalar dependencias Python**:
   ```bash
   pip install pydantic>=2.0.0
   ```

4. **Actualizar lista de aplicaciones** en Odoo

5. **Instalar módulos** en el siguiente orden:
   - `llm` (base)
   - `llm_tool` 
   - `llm_mcp` (opcional)

## Configuración

### 1. Configurar Proveedor LLM

1. Ir a **LLM → Configuración → Proveedores**
2. Crear nuevo proveedor (ej: OpenAI, Anthropic)
3. Configurar API key y parámetros
4. Hacer clic en "Fetch Models" para importar modelos

### 2. Configurar Herramientas

1. Ir a **LLM → Configuración → Herramientas**
2. Las herramientas predefinidas se crean automáticamente
3. Configurar permisos y consentimientos según necesidades

## Uso

### Herramientas Predefinidas

- **`odoo_record_retriever`**: Buscar y recuperar registros
- **`odoo_record_creator`**: Crear nuevos registros
- **`odoo_record_updater`**: Actualizar registros existentes
- **`odoo_record_unlinker`**: Eliminar registros
- **`odoo_model_inspector`**: Inspeccionar estructura de modelos
- **`odoo_model_method_executor`**: Ejecutar métodos de modelos

### Ejemplo de Uso

```python
# En un modelo personalizado
class MyModel(models.Model):
    _name = 'my.model'
    _inherit = ['mail.thread']
    
    def chat_with_ai(self, message):
        # Obtener herramientas disponibles
        tools = self.env['llm.tool'].search([('active', '=', True)])
        
        # Obtener modelo LLM
        model = self.env['llm.model'].search([('default', '=', True)], limit=1)
        
        # Enviar mensaje con herramientas
        response = model.chat(
            messages=[{'role': 'user', 'content': message}],
            tools=tools
        )
        
        return response
```

## Diferencias con Odoo 16

| Aspecto | Odoo 16 | Odoo 15 |
|---------|---------|---------|
| Campos JSON | `fields.Json()` | `fields.Text()` + parsing manual |
| Widgets JSON | `json_editor`, `json_inline` | `text` |
| Almacenamiento | JSON nativo | JSON como string en Text |
| Performance | Nativo | Parsing overhead mínimo |

## Migración desde Odoo 16

Si tienes datos existentes en Odoo 16, las migraciones incluidas se encargarán automáticamente de:

1. Convertir campos JSON a Text con contenido JSON
2. Migrar mensajes de herramientas al nuevo formato
3. Preservar toda la funcionalidad existente

## Soporte

- **Documentación**: Ver archivos README individuales de cada módulo
- **Issues**: Reportar problemas en el repositorio
- **Contribuciones**: Pull requests bienvenidos

## Licencia

LGPL-3 - Ver archivos de licencia individuales en cada módulo.

---

*© 2025 Apexive Solutions LLC. Adaptado para Odoo 15.*