# 📘 Proyecto de Normalización de Bases de Datos
**Autor:** David Alejandro Anzola Caicedo  

Este proyecto desarrolla un proceso completo de análisis, diseño y normalización de bases de datos en tres etapas:  
1) Análisis conceptual  
2) Caso práctico aplicado a "Fred’s Furniture"  
3) Proyecto personal de modelado E–R

---

# ✅ Parte 1: Análisis conceptual – Devart

A continuación se presentan las respuestas a las cinco preguntas del análisis conceptual:

### **1. ¿Qué es la normalización y por qué es necesaria?**  
La normalización es el proceso de organizar los datos en una base para reducir redundancia y mejorar integridad.  
Evita datos duplicados, inconsistencias y garantiza que la base pueda escalar correctamente.

### **2. ¿Qué son las dependencias funcionales?**  
Son relaciones donde un atributo determina a otro.  
EJ: si *CódigoProducto → NombreProducto*, entonces el código determina el nombre.  
Identificarlas es clave para separar correctamente entidades y relaciones.

### **3. ¿Qué problemas evita la 1FN, 2FN y 3FN?**  
- **1FN:** elimina grupos repetitivos y celdas con múltiples valores.  
- **2FN:** elimina dependencias parciales en claves compuestas.  
- **3FN:** elimina dependencias transitivas (atributos que dependen de otros no clave).

### **4. ¿Por qué las tablas no deben mezclar datos de distintas entidades?**  
Porque causa redundancia, dificulta actualizaciones e incrementa errores.  
Cada entidad debe tener su propia tabla para mantener integridad y claridad.

### **5. ¿Qué ventajas generales aporta un modelo normalizado?**  
- Menos duplicidad  
- Mayor consistencia  
- Integridad referencial  
- Tablas más eficientes  
- Relaciones claras entre entidades  
- Mejor rendimiento en consultas complejas  

---

# ✅ Parte 2: Caso Fred’s Furniture

## 🪑 Descripción del caso
Fred’s Furniture tenía registros en una sola tabla mezclando pedidos, clientes, muebles y proveedores.  
Esto generaba duplicidad, datos inconsistentes y dificultades para consultar información.

El objetivo fue **analizar la tabla, identificar dependencias funcionales y normalizarla hasta obtener un modelo E-R limpio y funcional.**

---

# ✅ Retos y resultados

### **Reto 1: Exceso de redundancia en los datos**
La tabla repetía información de clientes, muebles y pedidos múltiples veces.  

✅ *Solución:* identificar atributos repetidos y separarlos en entidades individuales.  
✅ *Resultado:* se crearon tablas independientes como **Cliente**, **Producto**, **Pedido**, etc.

---

### **Reto 2: Dependencias funcionales incorrectas**
Había atributos que dependían de otros no clave, por ejemplo la dirección del cliente dependía del número de pedido.  

✅ *Solución:* clasificar atributos por dependencia directa a claves primarias.  
✅ *Resultado:* eliminación de dependencias parciales y transitivas (2FN y 3FN logradas).

---

### **Reto 3: Falta de integridad referencial**
Al tener todo en una sola tabla, no existían relaciones formales entre entidades.  

✅ *Solución:* creación de llaves primarias y foráneas.  
✅ *Resultado:* consistencia y claridad entre pedidos, clientes y productos.

---

# ✅ Código SQL representativo

```sql
CREATE TABLE cliente (
    id_cliente INT PRIMARY KEY,
    nombre VARCHAR(80),
    telefono VARCHAR(20),
    direccion VARCHAR(120)
);

CREATE TABLE producto (
    id_producto INT PRIMARY KEY,
    nombre VARCHAR(80),
    categoria VARCHAR(50),
    precio DECIMAL(10,2)
);

CREATE TABLE pedido (
    id_pedido INT PRIMARY KEY,
    fecha DATE,
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente)
);

CREATE TABLE detalle_pedido (
    id_detalle INT PRIMARY KEY,
    id_pedido INT,
    id_producto INT,
    cantidad INT,
    FOREIGN KEY (id_pedido) REFERENCES pedido(id_pedido),
    FOREIGN KEY (id_producto) REFERENCES producto(id_producto)
);
