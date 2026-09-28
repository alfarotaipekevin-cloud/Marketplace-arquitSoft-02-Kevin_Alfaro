# Registros de Decisiones Arquitectónicas (ADR)

| ID | Decisión Arquitectónica | Driver Relacionado | Justificación | Resultado |
| :--- | :--- | :--- | :--- | :--- |
| **ADR-001** | **Monolito Modular** | DA01, DA06 | Organizar las funcionalidades en módulos independientes dentro de una misma aplicación desplegable[cite: 1]. | Módulos de Catálogo, Carrito, Pedidos, Pagos y Usuarios[cite: 1]. |
| **ADR-002** | **Clean Architecture** | DA06 | Separar las reglas de negocio de los detalles tecnológicos externos[cite: 1]. | Capas de Dominio, Aplicación, Adaptadores e Infraestructura[cite: 1]. |
| **ADR-003** | **Estrategia de Caché** | DA02 | Reducir consultas repetitivas a la base de datos relacional[cite: 1]. | Caché para información de consulta frecuente en el catálogo[cite: 1]. |
| **ADR-004** | **Integración de Pagos mediante Adaptadores** | DA04 | Desacoplar los casos de uso del proveedor de pagos específico[cite: 1]. | Contrato/interfaz de pagos y adaptadores para pasarelas externas[cite: 1]. |