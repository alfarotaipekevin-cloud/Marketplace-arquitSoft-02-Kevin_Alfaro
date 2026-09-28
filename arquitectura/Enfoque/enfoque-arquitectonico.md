# Enfoque Arquitectónico: Clean Architecture (Arquitectura Limpia)

## 1. Resumen del Enfoque Arquitectónico

| Elemento | Descripción Aplicada al Marketplace |
| :--- | :--- |
| **Patrón / Enfoque** | Clean Architecture (Arquitectura Limpia)[cite: 1]. |
| **Objetivo** | Separar responsabilidades y controlar las dependencias hacia el dominio[cite: 1]. |
| **¿Qué problema resuelve?** | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas, como bases de datos, API y servicios de pago[cite: 1]. |
| **Capas definidas** | Presentación, Aplicación, Dominio e Infraestructura[cite: 1]. |
| **Beneficios** | • Facilita el mantenimiento y las pruebas unitarias[cite: 1].<br>• Permite cambiar implementaciones técnicas sin modificar innecesariamente las reglas del negocio[cite: 1].<br>• Mejora la organización y separación de responsabilidades del código[cite: 1]. |

## 2. Definición de Capas Internas y Responsabilidades

1. **Capa de Dominio (Domain Layer):**
   - Es el núcleo de la aplicación y no depende de ningún elemento externo[cite: 1].
   - Contiene las entidades fundamentales del negocio (`Producto`, `Carrito`, `Pedido`)[cite: 1].
   - Define las reglas de negocio y los contratos/interfaces (`RepositorioProductos`, `IntegracionPagos`, `NotificacionCorreo`)[cite: 1].

2. **Capa de Aplicación (Application Layer / Use Cases):**
   - Contiene la lógica de la aplicación organizada en Casos de Uso (`ConsultarCatalogoUseCase`, `AgregarAlCarritoUseCase`, `EjecutarCompraUseCase`)[cite: 1].
   - Orquesta el flujo de datos apoyándose únicamente en la capa de Dominio[cite: 1].

3. **Capa de Presentación (Presentation Layer):**
   - Aloja los componentes de la interfaz de usuario en Angular (`CatalogoComponent`, `DetalleCarrito`, `CarritoComponent`, `AppComponent`)[cite: 1].
   - Se encarga del renderizado y la captura de eventos del usuario[cite: 1].

4. **Capa de Infraestructura (Infrastructure Layer):**
   - Agrupa las implementaciones técnicas concretas de los contratos definidos por el Dominio (`ServicioProductosHttp`, `RepositorioProductosMemoria`, `IntegracionPagosStripe`, `ServicioCorreoSimulado`)[cite: 1].
   - Gestiona la comunicación con servicios y API REST externos[cite: 1].

## 3. Reglas de Dependencia
- Las dependencias internas apuntan estrictamente **hacia adentro** (hacia la capa de Dominio)[cite: 1].
- El Dominio no conoce detalles de la Infraestructura ni de la Presentación[cite: 1].
- La Infraestructura implementa las interfaces declaradas en el Dominio para permitir la inversión de dependencias[cite: 1].

## 4. Diagrama de Clean Architecture

```mermaid
graph LR
    subgraph UI[Presentación - Angular UI]
        direction TB
        CatComp[CatalogoComponent]
        DetCart[DetalleCarrito]
        CartComp[CarritoComponent]
        AppComp[AppComponent]
    end

    subgraph App[Aplicación - Casos de Uso]
        direction TB
        UC1[ConsultarCatalogoUseCase]
        UC2[AgregarAlCarritoUseCase]
        UC3[EjecutarCompraUseCase]
    end

    subgraph Dom[Dominio - Núcleo de Negocio]
        direction TB
        Ent1[Entidad: Producto]
        Ent2[Entidad: Carrito]
        Ent3[Entidad: Pedido]
        IRepo[Interfaz: RepositorioProductos]
        IPay[Interfaz: IntegracionPagos]
        INotify[Interfaz: NotificacionCorreo]
    end

    subgraph Infra[Infraestructura y Adaptadores]
        direction TB
        HttpSvc[ServicioProductosHttp]
        MemRepo[RepositorioProductosMemoria]
        StripePay[IntegracionPagosStripe]
        EmailSvc[ServicioCorreoSimulado]
    end

    subgraph External[Sistemas Externos]
        BackendAPI[Servicio API REST Backend Marketplace]
    end

    UI --> App
    App --> Dom
    Infra -.->|Implementa interfaz| IRepo
    Infra -.->|Implementa interfaz| IPay
    Infra -.->|Implementa interfaz| INotify
    HttpSvc --> BackendAPI
    StripePay --> BackendAPI