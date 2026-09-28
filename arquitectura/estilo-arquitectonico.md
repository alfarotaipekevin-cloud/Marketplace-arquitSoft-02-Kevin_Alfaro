# Estilo Arquitectónico: Monolito Modular en Capas

## 1. Descripción del Estilo
Se ha seleccionado un estilo de **Monolito Modular con organización lógica en capas** para el backend Node.js/Express del Marketplace[cite: 1]. El sistema se ejecuta como una sola unidad de despliegue, pero sus componentes se encuentran delimitados por dominios funcionales (Usuarios, Sellers, Catálogo, Carrito, Pedidos)[cite: 1].

## 2. Justificación
- **Baja complejidad de despliegue:** Ideal para el volumen actual del negocio sin incurrir en la sobrecarga de infraestructura de microservicios[cite: 1].
- **Límites modulares claros:** Permite aislar responsabilidades y facilita una posible extracción futura de microservicios si un módulo requiere escalamiento independiente[cite: 1].
- **Estructura en capas:** Garantiza el orden interno agrupando el código en Presentación, Negocio y Datos[cite: 1].

## 3. Diagrama de Estilo Arquitectónico

```mermaid
graph TD
    Client[Cliente Web / Frontend Angular] -->|HTTPS / REST| MW[Middlewares Express: Auth JWT, CORS, Validaciones]

    subgraph Monolith[Monolito Marketplace Backend Node.js / Express]
        
        subgraph Layer1[1. Capa de Presentación]
            UsersCtrl[Módulo Usuarios: controller / routes]
            SellersCtrl[Módulo Sellers: controller / routes]
            CatalogCtrl[Módulo Catálogo: controller / routes]
            CartCtrl[Módulo Carrito: controller / routes]
            OrdersCtrl[Módulo Pedidos: controller / routes]
        end

        subgraph Layer2[2. Capa de Lógica de Negocio]
            UsersSvc[usuarios.service.js]
            SellersSvc[sellers.service.js]
            CatalogSvc[catalogo.service.js]
            CartSvc[carrito.service.js]
            OrdersSvc[pedidos.service.js]
        end

        subgraph Layer3[3. Capa de Datos]
            UsersRepo[usuarios.repository.js]
            SellersRepo[sellers.repository.js]
            CatalogRepo[catalogo.repository.js]
            CartRepo[carrito.repository.js]
            OrdersRepo[pedidos.repository.js]
            DataAccess[Acceso a Datos Compartido: ORM Sequelize]
        end
    end

    MW --> Layer1
    
    UsersCtrl --> UsersSvc
    SellersCtrl --> SellersSvc
    CatalogCtrl --> CatalogSvc
    CartCtrl --> CartSvc
    OrdersCtrl --> OrdersSvc

    UsersSvc --> UsersRepo
    SellersSvc --> SellersRepo
    CatalogSvc --> CatalogSvc
    CartSvc --> CartSvc
    OrdersSvc --> OrdersRepo

    UsersRepo --> DataAccess
    SellersRepo --> DataAccess
    CatalogRepo --> DataAccess
    CartRepo --> DataAccess
    OrdersRepo --> DataAccess

    DataAccess --> DB[(PostgreSQL Database)]
    OrdersSvc -->|HTTPS / REST| PaymentExt[Pasarela de Pagos Stripe / PayPal]
    OrdersSvc -->|HTTPS / REST| ShippingExt[Servicio de Envíos DHL / Olva]