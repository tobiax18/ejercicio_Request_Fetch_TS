# 🌐 The Universal Data Fetcher

Sistema genérico en TypeScript para consumir cualquier API REST de forma reutilizable y segura.

---

## 📋 Descripción

Este proyecto implementa un sistema de fetching de datos completamente tipado con TypeScript. Usando **genéricos**, se construyó una arquitectura reutilizable que permite consumir cualquier endpoint REST sin repetir lógica.

La API utilizada es [JSONPlaceholder](https://jsonplaceholder.typicode.com), una API pública gratuita para pruebas.

---

## 🏗️ Arquitectura

El proyecto se divide en tres capas principales:

### 1. Interfaz `ApiResponse<T>`
Representa cualquier respuesta del sistema de forma estandarizada.

```typescript
interface ApiResponse<T> {
  data: T | null;       // Cuerpo de la respuesta si fue exitosa
  error: string | null; // Mensaje de error si algo falló
  status: number;       // Código HTTP (200, 404, 500, etc.)
}
```

### 2. Función `apiRequest<T>`
Función genérica que realiza el `fetch`, valida la respuesta y maneja dos tipos de errores:

- **Error HTTP**: el servidor respondió pero con un código de error (ej. 404, 500). Se detecta con `response.ok`.
- **Error de red**: no hubo respuesta del servidor (sin internet, servidor caído).

```typescript
async function apiRequest<T>(url: string): Promise<ApiResponse<T>>
```

### 3. Clase `ApiService<T>`
Encapsula el acceso a un recurso específico de la API. Recibe el endpoint base en el constructor y expone dos métodos reutilizables.

```typescript
class ApiService<T> {
  constructor(private endpoint: string) {}
  async getAll(): Promise<ApiResponse<T[]>>
  async getOne(id: number): Promise<ApiResponse<T>>
}
```

---

## 🚀 Instalación y uso

### Requisitos
- Node.js 18+
- npm

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/TU_USUARIO/Request_Fetch_TS.git
cd Request_Fetch_TS

# 2. Instalar dependencias
npm install

# 3. Ejecutar el proyecto
npm run dev
```

---

## 🧪 Pruebas realizadas

Se definieron dos tipos de datos y se probaron los métodos de `ApiService`:

```typescript
type Post = { id: number; title: string; body: string; userId: number };
type User = { id: number; name: string; email: string; phone: string };

const postService = new ApiService<Post>('https://jsonplaceholder.typicode.com/posts');
const userService = new ApiService<User>('https://jsonplaceholder.typicode.com/users');
```

| Prueba | Resultado |
|--------|-----------|
| `postService.getAll()` | ✅ 200 — retorna 100 posts |
| `postService.getOne(1)` | ✅ 200 — retorna el post #1 |
| `userService.getAll()` | ✅ 200 — retorna 10 usuarios |
| `postService.getOne(99999)` | ✅ 404 — error manejado correctamente |

---

## 📁 Estructura del proyecto

```
Request_Fetch_TS/
├── src/
│   └── index.ts       # Código principal
├── .gitignore
├── package-lock.json
├── package.json
├── tsconfig.json
└── README.md
```

---

## 🛠️ Tecnologías

- [TypeScript](https://www.typescriptlang.org/)
- [tsx](https://github.com/privatenumber/tsx) — ejecutor de TypeScript sin compilar
- [JSONPlaceholder](https://jsonplaceholder.typicode.com/) — API pública para pruebas
- Fetch API nativa

---

## 📚 Conceptos aplicados

- **Genéricos en TypeScript** (`<T>`) para crear código reutilizable con cualquier tipo
- **Interfaces genéricas** para estandarizar respuestas
- **Clases genéricas** para encapsular lógica de acceso a datos
- **Manejo de errores** diferenciando errores HTTP vs errores de red
- **async/await** con `try/catch` para operaciones asíncronas

---

## 📄 Licencia

Este proyecto es para fines educativos.
