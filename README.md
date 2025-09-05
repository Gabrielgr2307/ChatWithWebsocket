# ChatWithWebsocket
# Chat en Tiempo Real con React Native y NestJS

Esta es una aplicación de chat en tiempo real construida con **React Native** para el frontend y **NestJS** con **Socket.io** para el backend. La aplicación permite a los usuarios unirse a un chat público, ver quiénes están en línea y enviar mensajes privados a otros usuarios.

## Características

-   **Chat Público y Privado:** Envía mensajes a todos los usuarios o mantén conversaciones uno a uno.
-   **Lista de Usuarios en Línea:** Ve en tiempo real quiénes están conectados en el chat.
-   **Diseño Responsivo:** La interfaz se adapta a diferentes tamaños de pantalla, ofreciendo una vista de cajón en móviles y una vista de panel lateral en web.
-   **Mensajes Dinámicos:** Los mensajes propios se alinean a la derecha, mientras que los de otros usuarios se alinean a la izquierda.

## Tecnologías Utilizadas

### Frontend
-   **React Native**
-   **Expo**
-   **Socket.io Client**
-   **Lucide React Native** para iconos

### Backend
-   **NestJS**
-   **Socket.io**
-   **TypeScript**

## Requisitos Previos

Asegúrate de tener instalados los siguientes programas:
-   Node.js
-   npm o yarn
-   Expo CLI
-   Un gestor de bases de datos para tu backend (si es necesario)

## Instalación y Configuración

Sigue estos pasos para poner en marcha el proyecto:

### 1. Configuración del Backend

1.  Clona el repositorio:
    ```bash
    git clone [https://github.com/tu-usuario/tu-repositorio.git](https://github.com/tu-usuario/tu-repositorio.git)
    cd tu-repositorio/backend
    ```
2.  Instala las dependencias:
    ```bash
    npm install
    # o
    yarn install
    ```
3.  Inicia el servidor de NestJS:
    ```bash
    npm run start:dev
    # o
    yarn start:dev
    ```
    El servidor se ejecutará en `http://localhost:3000`.

### 2. Configuración del Frontend

1.  Ve al directorio del frontend:
    ```bash
    cd ../frontend
    ```
2.  Instala las dependencias:
    ```bash
    npm install
    # o
    yarn install
    ```
3.  Asegúrate de que la URL del servidor en `src/services/socketService.ts` apunte a tu dirección IP local si estás usando un dispositivo físico para probar (por ejemplo, `http://192.168.1.10:3000`).

    ```typescript
    // src/services/socketService.ts
    // Reemplaza 'localhost' con tu IP local si es necesario
    await this.socket.connect('http://tu-ip-local:3000'); 
    ```

4.  Inicia la aplicación de Expo:
    ```bash
    npm start
    # o
    yarn start
    ```
    Esto abrirá un código QR. Escanéalo con la aplicación de Expo Go en tu dispositivo móvil o presiona `w` para abrirlo en el navegador web.

## Estructura del Proyecto
