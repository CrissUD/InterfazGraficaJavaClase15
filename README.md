# Interfaz Gráfica en Java

Curso propuesto por el grupo de trabajo Semana de Ingenio y Diseño (**SID**) de la Universidad Distrital Francisco Jose de Caldas.

## Monitor

**Cristian Felipe Patiño Cáceres** - Estudiante de Ingeniería de Sistemas de la Universidad Distrital Francisco Jose de Caldas

# Clase 15

## Objetivos

* Reconocer el modo de utilizar el enfoque de posicionamiento a traves de LayoutManager e identificar las ventajas de utilizar este enfoque de posicionamiento.
* Identificar alguno de los principales tipos de Layout para gestionar el posicionamiento de objetos gráficos.
* Realizar uso del GridBadLayout dentro de nuestro proyecto y reconocer cada uno de los procesos que intervienen en su construcción.
* Explicar particularidades en torno al uso de GridBadLayout para entender a detalle como funciona este LayoutManager.

# Antes de Comenzar

Antes de iniciar con la clase de hoy vamos a realizar una actualización en la carpeta **images** de nuestro proyecto, en este caso vamos a agregar nuevas imágenes sobre productos que están contenidas en una carpeta internas llamada **productos**, usted puede descargar estas imágenes dentro del mismo repositorio entrando a la carpeta **Clase15/resources/images/productos**:
<div align='center'>
    <img  src='https://i.imgur.com/eRm1Uak.png'>
    <p>Imágenes sobre productos.</p>
</div>

***Nota: Estas imágenes fueron tomadas de la página [ed.team](https://ed.team/) son ellos los creadores y dueños de estas imágenes, aquí daremos uso a estas imágenes con motivos académicos. Por favor utilizar estas imágenes con responsabilidad en caso de ser descargadas.***

Vamos a crear algunos nuevos objetos decoradores dentro de nuestro servicio **RecursosService** ya que vamos a necesitar para la construcción de nuestra interfaz en esta sesión, primero vamos a crear dos nuevos colores de tipo azul:
* **Declaración:**
```javascript
// Dentro del servicio RecursosService
private Color colorAzulClaro, colorAzulMarino;
```

* **Ejemplificación**
```javascript
// Dentro del constructor
colorAzulClaro = new Color(231, 244, 253);
colorAzulMarino = new Color(17, 146, 238);
```

* **Métodos get:**
```javascript
public Color getColorAzulClaro(){
    return colorAzulClaro;
}

public Color getColorAzulMarino(){
    return colorAzulMarino;
}
```

También vamos a crear una fuente que usaremos en los productos que se verán a lo largo de esta clase:
* **Declaración:**
```javascript
private Font fontTProducto;
```

* **Ejemplificación:**
```javascript
fontTProducto = new Font("LuzSans-Book", Font.BOLD, 28);
```

* **Método Get:**
```javascript
public Font getFontTProducto(){
    return fontTProducto;
}
```

Recordando un poco nuestro recorrido, en la clase anterior vimos el funcionamiento de cada método contenido en el servicio **GraficosAvanzadosService** para personalizar nuestros objetos gráficos de forma especial.

<div align='center'>
    <img  src='https://i.imgur.com/rJV3RFt.png'>
    <p>Uso del servicio Gráficos Avanzados para dar un borde circular y bordes difuminados</p>
</div>

# Posicionamiento de elementos con LayoutManager

En esta sesión se explicara el uso de **LayoutManager** para el posicionamiento de nuestros objetos gráficos en la interfaz y para una explicación completa veremos los siguientes items:
* **Definición de LayoutManager**.
* **Tipos de LayoutManager**.
* **Preparación de proyecto para uso de GridBadLayout**.
* **Implementación de GridBadLayout**.

# Definición de LayoutManager

Los **LayoutManager** son objetos en Java encargados de posiciónar los objetos gráficos dentro de nuestras interfaces gráficas de forma automática y bajo ciertos criterios existen varios tipos con distintas configuraciones pero todos ofrecen el servicio de posicionar elementos en pantalla y darles un tamaño adecuado. 

Hasta el momento no hemos utilizado ningún **LayoutManager** y esto es por que son complejos de utilizar y aprender, pero el tiempo que nos ahorramos aprendiendo el uso de estos **Layouts** lo gastamos en el calculo de cada posición y tamaño de cada objeto gráfico en pantalla y muchas veces haciendo multiples pruebas para comprobar que nuestros elementos en pantalla están como queremos. Aunque esto trae cierta ventaja ya que nosotros somos los que controlamos este aspecto y podemos posicionar nuestros elementos como queremos si es verdad que el tiempo invertido para esto en cada objeto es grande. 

El uso de **LayoutManager** ademas de ahorrarnos tiempos de cálculos para posicionar nuestros elementos tiene un serie de ventajas que valen la pena recalcar.

## Reactividad

Esta es quizás la ventaja que mas resalta en el uso de **LayoutManager** a comparación del uso de **posicionamiento por pixeles**, cuando usamos el segundo enfoque estamos dando un **tamaño y posición fija** a nuestros elementos y esto quiere decir que si la ventana se va a cambiar de tamaño en algún punto estos objetos no van a reaccionar al cambio de la pantalla y se quedaran estáticos con la posición y tamaño dado. Por el contrario cuando utilizamos **LayoutManager** nuestros objetos gráficos van a tener un **tamaño y posición dinámica** que va a depender del tamaño de la ventana y se va a reactivar una ves este cambie para que el tamaño y posición de este objeto sea proporcional al nuevo tamaño. 

<div align='center'>
    <img  src='./gifs/sinLayout.gif'>
    <p>Prueba de cambio de tamaño de ventana sin LayoutManager</p>
</div>

<div align='center'>
    <img  src='./gifs/conLayout.gif'>
    <p>Prueba de cambio de tamaño de ventana utilizando LayoutManager</p>
</div>

En los ejemplos anteriores tenemos un botón que queremos que este ubicado en el medio de nuestra ventana siempre, sin embargo cuando cambiamos el tamaño de la ventana este no reacciona a menos que usemos un **LayoutManager**.

Esto se podría realizar también con un enfoque derivado del **Posicionamiento por pixeles** que es el **Posicionamiento por porcentajes**, pero de todos modos nosotros internamente tendríamos que crear métodos que de forma manual cambiaran el tamaño y posición de nuestros elementos una vez la ventana cambia de tamaño lo cual requiere más lineas de código mientras que el uso de **LayoutManager** hace que este proceso se realice de forma automática.

## Tamaño real a nuestros objetos gráficos

Esto es un problema que muchos desarrolladores no pueden solucionar cuando utilizan **Posicionamiento por pixeles** y es que cuando se quiere utilizar un **JScrollPane** debido a que lo elementos dentro de la ventana van a ocupar un tamaño mayor a esta, tenemos que utilizar **ScrollBar** para poder navegar dentro de la pantalla y asi ver todos los elementos.

El problema aquí radica en que si utilizamos el enfoque que hemos venido utilizando el **JScrollPane** no va a reconocer un tamaño real en nuestros elementos y por esta razón no va a activar los **ScrollBar** para navegar. Esto quiere decir que si por ejemplo tenemos un botón que ocupa de ancho mas de  lo que la ventana puede mostrar nuestro **JScrollPane** no nos activara estas barras y no podremos ver el objeto gráfico completo. A esto le llamamos un **Tamaño Falso** por que aunque le configuremos este tamaño a nuestro botón y podamos ver el tamaño en pantalla internamente Java no lo esta reconociendo y por eso no activará barras de navegación.

Por otro lado usando **LayoutManager** el compilador es quien se encarga de dar un tamaño y por esto los tamaños serán **reales** para el programa y activará las barras de navegación.

<div align='center'>
    <img  src='https://i.imgur.com/iQZQacH.png'>
    <p>Botón con un tamaño mayor sin LayoutManager</p>
</div>

<div align='center'>
    <img  src='https://i.imgur.com/plaQwkC.png'>
    <p>Botón con un tamaño mayor usando LayoutManager</p>
</div>

Este problema se puede solucionar mediante el uso del método **setPreferredSize** y el uso de objetos **Dimension** y asi Java podrá reconocer el **Tamaño Real** de nuestros objetos Gráficos y activar las barras de navegación en un **JScrollPane** cuando usemos **Posicionamiento por pixeles**. Sin embargo esto implica crear un objeto **Dimension** nuevo en memoria por cada objeto gráfico en nuestra interfaz lo cual llenará mas la memoria y habrá mas consumos de recursos.

## Tamaño dinámico de la ventana

Cuando utilizamos **posicionamiento por pixeles** siempre debemos estar pendiente del tamaño ideal de nuestra ventana mientras realizamos su contenido, sin embargo esto queda de lado usando **LayoutManager**, una vez nuestros objetos están creados y agregados, solo basta con utilizar el método **Pack()** de la ventana y esta ajustara su tamaño para que todos los elementos en pantalla puedan verse de manera adecuada.

***

Los **LayoutManager** posiciónan los elementos bajo ciertos criterios y depende del **Layout** que vamos a manejar en la siguiente sección se explicaran las características generales de los **LayoutManager** mas importantes.

# Tipos de LayoutManager

## BorderLayout

Este **LayoutManager** es el que viene por defecto en la mayoría de objetos encargados de contener otro objetos tales como **Frames, Paneles, ScrollPane** etc. Divide la ventana en 5 partes: **centro, arriba, abajo, derecha e izquierda** y esto implica cierta estructura:

* Los objetos que se ubiquen en la parte superior o inferior va a ocupar siempre todo el ancho de la ventana.
* Los objetos que se ubiquen en los laterales ocuparan siempre todo el alto de la ventana.
* El objeto ubicado en el centro tratará siempre de expandirse en ambas dimensiones, de esta forma si hay un único elemento central ocupara toda la ventana, de lo contrario se extenderá hasta el limite con sus objetos compañeros.

El **BorderLayout** es ideal para posicionar ventanas ya que tiene una estructura básica de una aplicación de escritorio. Es común que los frames utilicen este **LayoutManager** para dar la estructura principal de la aplicación y cada elemento en especifico use sus propios layouts.

A continuación veremos un ejemplo sencillo:

<div align='center'>
    <img  src='https://i.imgur.com/EA1AkvL.png'>
    <p>Implementación BorderLayout</p>
</div>

<div align='center'>
    <img  src='https://i.imgur.com/xhq1B8G.png'>
    <p>Resultado implementación BorderLayout</p>
</div>

Podemos ver algunas particularidades:
* **Construcción Layout**: Basta con ejemplificarlos y agregarlo mediante el método **setLayout**, en este caso al ser una ventana este paso esta de mas ya que por defecto tiene este layout, pero se coloco para mostrar su construcción.
* **Dirección de Objetos Gráficos**: Para indicarle a nuestra aplicación debemos indicar la dirección del objeto una vez se agrega a la ventana mediante el objeto **BorderLayout** y sus opciones **North, South, West, East, Center**.
* **Tamaño Objetos:** Por defecto el compilador ira agrandando o reduciendo la ventana a medida que cada panel ubicado va agregando objetos, en este caso solo se crearon los paneles asi que para representar que ocuparan un tamaño usamos el método **setPreferredSize** pero esto no será necesario una vez tenga elementos internos.
* **Tamaño de la ventana**: No es necesario indicarle un tamaño fijo a la ventana, mediante el método **pack()** la ventana encontrará el menor tamaño para que los objetos gráficos quepan en ella.

Como desventaja esta el hecho de imponer una sola estructura para crear aplicaciónes, si queremos dar otra forma a nuestra aplicación no será posible.

## FlowLayout

Este **LayoutManager** coloca los objetos gráficos de forma horizontal, creando una fila dentro de la ventana. De esta forma el layout posicióna los elementos de igual forma a lo largo del eje X. 
Ademas trae ciertas ventajas, por ejemplo si la ventana tiene un ancho muy grande y sobra espacio tratara siempre de ubicar los elementos de forma horizontal y centrada. Por otro lado si la ventana no ocupa el ancho suficiente para colocar todos los elementos el **LayoutManager** automáticamente crea una fila debajo para poder ubicar los objetos gráficos sobrantes y siempre tratando de centrar los elementos.
Es ideal para usarse por ejemplo en barras de herramientas, barras de títulos, menus horizontales etc.

A continuación se muestra un ejemplo simple de como usar este **FlowLayout**:
<div align='center'>
    <img  src='https://i.imgur.com/XTq9zDz.png'>
    <p>Implementación de FlowLayout</p>
</div>

<div align='center'>
    <img  src='https://i.imgur.com/l0eSXcv.png'>
    <p>Resultado del código</p>
</div>

En este caso se creo una barra de titulo o **Header** bastante sencillo a partir del uso de **FlowLayout**. Noten que nosotros al crear nuestros objetos gráficos no estamos proporcionando ningún tamaño ni posición ya que de eso se va a encargar automáticamente el **LayoutManager** el resto del proceso es igual al que hemos manejado.

Aunque es un Layout ideal para realizar este tipo de cosas tiene ciertas desventajas:
* **Posicionamiento Horizontal:** Solo funciona para posicionar de manera horizontal, no podemos controlar la posición de forma vertical.
* **Espacio entre objetos gráficos:** Este layout por defecto pega todos los componentes y los tratará de centrar, si queremos simular espaciado entre objetos gráficos debemos realizar algunas configuraciones adicionales como usar **bordes vacíos**.
* **Alineación Vertical Automática:** No es posible realizar una alineación vertical entre elementos de forma manual, si un elemento es mas alto que el resto, el **FlowLayout** tratara siempre de centrarlos de forma vertical pero si queremos que alguno de esos este en la parte superior o inferior de la fila no será posible.

## BoxLayout

Este **LayoutManager** esta creado dentro de la librería **Swing** y cubre la necesidad de poder posicionar elementos de forma vertical, pero al igual que su antecesor si se decea, se puede posicionar elementos de forma horizontal lo cual hace de este **LayoutManager** un objeto mas completo para posicionar elementos.

Puede usarse para menus verticales, menus horizontales, combinaciones entre posiciones, barras títulos, barras de herramientas etc.

A continuación se muestra un ejemplo de uso:
<div align='center'>
    <img  src='https://i.imgur.com/woKFcKK.png'>
    <p>Implementación BoxLayout</p>
</div>

<div align='center'>
    <img  src='https://i.imgur.com/VJjL2As.png'>
    <p>Resultado de código</p>
</div>

Del anterior ejemplo se pueden notar varias cosas:
* **Creación BoxLayout**: Para crear este **LayoutManager** se debe pasar por parámetro le objeto gráfico que contendrá el Layout y ademas la orientación siento:
    * **X_AXIS**: para posicionar elementos de forma Horizontal
    * **Y_AXIS**: para posicionar elementos de forma Vertical.
    * **LINE_AXIS o PAGE_AXIS**: Le da la responsabilidad a cada objeto gráfico de indicar si se posicionará deforma horizontal o vertical de acuerdo al anterior objeto agregado.
* **Espacio entre elementos**: El **BoxLayout** maneja unos elementos especiales para realizar la separación entre elementos siendo:
    * **RigidArea**: Un elemento que ocupa un espacio con una dimensión y que se encarga de separar los elementos.
    * **HorizontalGlue**: Crea la separación maxima entre elementos de forma horizontal, es util cuando se quiere dos elementos en cada esquina horizontal. 
    * **RigidArea**: Un elemento que ocupa un espacio con una dimensión y que se encarga de separar los elementos.
    * **VerticalGlue**: Crea la separación maxima entre elementos de forma Vertical, es util cuando se quiere dos elementos en cada esquina Vertical. 
    * **Filler**: Este elemento crea 3 dimensiones por defecto para denotar cual sera la menor distancia, la distancia por defecto y la mayor distancia entre elementos en ambos ejes. Es el mas util para configurar espacios dinámicos a medida que el tamaño de la ventana cambie.
* **Alineación:** A diferencia de su antecesor los objetos gráficos dentro del panel con el **BoxLayout** pueden alinearse a nuestro antojo ya sea de forma vertical u horizontal. Debemos llamar al objeto **Component** de java y proporcionar la alineación que queremos. Sin embargo esta siempre suele dar problemas.
* **Combinación de Layouts**: Por supuesto se puede usar varios **Layouts** para distintos objetos gráficos por ejemplo en este caso la ventana no maneja ningún Layout, mientras que el panel **pContenido** si tiene el **BoxLayout**.

## GridLayout

Este **LayoutManger** permite colocar elementos dentro de una matriz lo cual nos da la posibilidad de posicionar objetos gráficos de manera horizontal y vertical al mismo tiempo. Con este Layout puedes proporcionar el numero de filas y columnas donde estarán contenidos los elementos. Muchas veces se suele usar **GridLayout** para mostrar una colección de elementos del mismo tipo y bajo unas mismas condiciones.

Puede ser usado para crear teclados virtuales, crear sudokus, sopas de leras, crear tarjetas con muestra de productos etc.
A continuación se muestra un ejemplo:

<div align='center'>
    <img  src='https://i.imgur.com/0BOVz6Z.png'>
    <p>Implementación GridLayout</p>
</div>

<div align='center'>
    <img  src='https://i.imgur.com/WDlbmPv.png'>
    <p>Resultado código GridLayout</p>
</div>

En el anterior ejemplo se representó una sopa de letras con ayuda de un **GridBagLayout**, podemos notar varias particularidades:
* **Construcción Layout**: para construir un GridLayout podemos pasar 4 parámetros.
    * Numero Filas.
    * Numero Columnas.
    * Espacio horizontal entre objetos gráficos.
    * Espacio vertical entre objetos gráficos.
* **Arreglo de Objetos Gráficos**: Como podemos ver es posible crear arreglos de tipo de algún objeto gráfico y de hecho esta es la forma mas común para agregar elementos a un **GridLayout**.

El uso de este Layout aunque nos da varias posibilidades para posicionar en ambas dimensiones tiene ciertas desventajas:
* **Mismo tamaño de elementos:** Cuando usamos **GridLayout** los elementos dentro de este van a ocupar todos el mismo tamaño, no es posible cambiar e tamaño de un solo elemento dentro del Grid.
* **Ocupación:** Cada elemento estrictamente ocupará una celda dentro del Grid, si quisiéramos que un elemento ocupe toda una fila o columna. Tampoco podemos hacer que un objeto ocupe dos o mas celdas.

## GridBadLayout

Este **LayoutManager** es el mas sofisticado y pretende cubrir todas las falencias de los demás Layouts, para empezar con este podemos posicionar elementos en ambas dimensiones (de forma vertical y horizontal), podemos realizar ademas una serie de configuraciones como:
* Establecer posición de un elemento dentro del grid de forma individual.
* Establecer el el espaciado entre objetos de forma individual y hacia cualquier dirección.
* Establecer cuantas celdas puede ocupar un elemento.
* Establecer la posición interna dentro de su celda.
* Establecer cuantas columnas tiene una fila por separado.

Esto da muchas posibilidades para crear interfaces como nosotros queremos y con distintas formas que con los anteriores Layout no es posible. En la siguiente sección vamos a implementar este **LayoutManager** en nuestro proyecto asi que no vamos a mostrar un pequeño ejemplo. Pero antes vamos a construir rápidamente los preparativos para usar nuestro **GridBadLayout**.

# Preparación de proyecto para uso de GridBadLayout

Queremos mostrar una seríe de productos en nuestra interfaz gráfica, en este caso los productos a mostrar serán una serie de cursos que nuestra empresa inventada dicta en linea, bueno esta información acerca de cada curso podría obtenerse de forma externa así que vamos a crear en esta sección todo lo necesario para que la aplicación pueda obtener, gestionar y mostrar esta información. 

Para empezar vamos a simular el contenido de la información mediante un archivo plano asi que sería bueno revisar la estructura de nuestro archivo, en este caso llamado **productos.txt** ubicado en nuestro paquete **archives**:
<div align='center'>
    <img  src='https://i.imgur.com/vJufXBS.png'>
    <p>Archivo plano creado</p>
</div>

Lo primero será entonces identificar de que se compone nuestro archivo plano:
<div align='center'>
    <img  src='https://i.imgur.com/CRjNoBX.png'>
    <p>Contenido en archivo plano productos.txt</p>
</div>

Podemos observar mediante este archivo plano que un curso se compone de:
* **id de Curso**.
* **Nombre de Curso**.
* **Descripción de Curso**.
* **Area de conocimiento de Curso**.
* **Puntuación de Curso**.
* **Dirección imágen de Curso**.

Ahora crearemos una **Clase Modelo** que encapsule toda esta información y de esta forma pode manejar los datos a traves de objetos:
<div align='center'>
    <img  src='https://i.imgur.com/LdNgIOR.png'>
    <p>Clase modelo que representa un producto</p>
</div>

Vamos a declarar sus correspondientes atributos junto a sus métodos **get y set**:

```javascript
public class Producto {
    private int id;
    private String nombreProducto, descripcion, campo, puntuacion;
    private ImageIcon imagen;

    public int getId(){
        return id;
    }

    public String getNombreProducto(){
        return  nombreProducto;
    }

    public String getDescripcion(){
        return  descripcion;
    }

    public String getCampo(){
        return  campo;
    }

    public String getPuntuacion(){
        return puntuacion;
    }

    public ImageIcon getImagen(){
        return imagen;
    }

    public void setId(int id){
        this.id = id;
    }

    public void setNombreProducto(String nombreProducto){
        this.nombreProducto = nombreProducto;
    }

    public void setDescripcion(String descripcion){
        this.descripcion = descripcion;
    }

    public void setCampo(String campo){
        this.campo = campo;
    }

    public void setPuntuacion(String puntuacion){
        this.puntuacion = puntuacion;
    }

    public void setImagen(ImageIcon imagen){
        this.imagen = imagen;
    }
}
```

Ahora vamos a crear un **Controlador externo** que simulara la gestión y obtención directa de la información de forma foránea:
<div align='center'>
    <img  src='https://i.imgur.com/GvHlM9Z.png'>
    <p>Creación Controlador de productos</p>
</div>

Vamos a declarar y ejemplificar nuestro arrayList que será la lista de los productos:
* **Declaración:**
```javascript
// Dentro del controlador ControlProductos
private ArrayList<Producto> productos;
```

* **Ejemplificación:**
```javascript
public ControlProductos(){
    productos = new ArrayList<Producto>();
}
```

Ahora vamos a crear el método encargado de cargar la información externa para ser contenida en la lista:
```javascript
public void cargarDatos(){
    File archivo = null;
    FileReader fr = null;
    BufferedReader br = null;
    try {
        archivo = new File ("Clase15/src/archives/productos.txt");
        fr = new FileReader(archivo);
        br = new BufferedReader(fr);
        String linea;
        while((linea=br.readLine())!=null){
            String[] atributos = linea.split(",");
            Producto producto = new Producto();
            producto.setId(Integer.parseInt(atributos[0]));
            producto.setNombreProducto(atributos[1]);
            producto.setDescripcion(atributos[2]);
            producto.setCampo(atributos[3]);
            producto.setPuntuacion(atributos[4]);
            producto.setImagen(new ImageIcon(atributos[5]));
            productos.add(producto);
        }
        fr.close(); 
    }
    catch(Exception e){
        e.printStackTrace();
    }
}
```

Finalmente vamos a devolver la lista de los productos para que sea el servicio quien gestione la información:

```javascript
public ArrayList<Producto> getProductos(){
    return productos;
}
```

Nuestro controlador esta listo, ahora vamos adentrarnos en nuestra aplicación frontend y vamos a crear un **Servicio** encargado de tomar esta información para ser gestionada:
<div align='center'>
    <img  src='https://i.imgur.com/SwOdUD5.png'>
    <p>Servicio ProductoService Creado.</p>
</div>

Primero crearemos la estructura básica del servicio:

* **Referencia estática a si mismo**
```javascript
private static ProductoService servicio;
```

* **Método para obtención única del servicio:**
```javascript
public static ProductoService getService(){
    if(servicio == null)
        servicio = new ProductoService();
    return servicio;
}
```

* **Método constructor**:
```javascript
public ProductoService(){
}
```

Ahora vamos a declarar y ejemplificar una referencia al controlador para obtener la información externa, adicionalmente vamos a declarar un arrayList para apuntar a la lista contenida en el controlador:
* **Declaración:**
```javascript
// Dentro del servicio ProductoService
private ControlProductos cProductos;
private ArrayList<Producto> productos;
```

* **Ejemplificación y obtención de información:**
```javascript
public ProductoService(){
    cProductos = new ControlProductos();
    productos = cProductos.getProductos();
}
```

Vamos a crear un método que nos retorne un producto de acuerdo a la posición que le enviemos:
```javascript
public Producto devolverProducto(int posicion){
    try{
        return productos.get(posicion);
    }
    catch(Exception e){
        return null;
    }
}
```

Ya tenemos todo listo en cuanto a la gestión de la información externa, ahora vamos a utilizar nuestro componente **Productos** pero antes de esto vamos a tener que realizar una **Reutilización de componentes** y para esto crearemos otro componente al cual llamaremos **Producto** y este se encargará de encapsular la estructura básica de cada producto, por ahora solo vamos a crearlo y a realizar su código básico:
<div align='center'>
    <img  src='https://i.imgur.com/qi1SveA.png'>
    <p>Creación del componente producto</p>
</div>

Vamos a crear el código base de la clase **ProductoComponent**:

* **Declaración de clase Template:**
```javascript
// Dentro de la clase ProductoComponent
private ProductoTemplate productoTemplate;
```

* **Creación de inyección:**
```javascript
public ProductoComponent(){
    productoTemplate = new ProductoTemplate(this);
}
```

* **Método get de clase Template:**
```javascript
public ProductoTemplate getProductoTemplate(){
    return productoTemplate;
}
```

Ahora en la clase **ProductoTemplate** vamos a extender de un JPanel, cerraremos la inyección y llamaremos a los servicios gráficos que podamos necesitar:

* **Declaraciones:**
```javascript
// Declaración Servicios y dependencias
private ProductoComponent productoComponent;
private ObjGraficosService sObjGraficos;
private RecursosService sRecursos;
```

* **Cerrando Inyección y obteniendo servicios:**
```javascript
public ProductoTemplate(ProductoComponent productoComponent){

    this.productoComponent = productoComponent;
    this.productoComponent.getClass();
    this.sObjGraficos = ObjGraficosService.getService();
    this.sRecursos = RecursosService.getService();
}
```

Con esto estamos listos para empezar a construir nuestros componentes a traves del **GridBagLayout**, en la siguiente sección vamos a ver a profundidad como utilizar este **LayoutManager** al tempo que vamos construyendo nuestro componente gráfico.

# Implementación de GridBadLayout

Vamos a configurar primero el componente padre en este caso **Productos** y en su clase **ProductosComponent** lo primero por hacer es obtener el servicio **ProductosService** para traer la información de los productos una vez se pidan:

* **Declaración:** 
```javascript
// Dentro de la clase ProductosComponent
private ProductoService sProducto;
```
* **Obtención del servicio:**
```javascript
public ProductosComponent(){
    sProducto = ProductoService.getService();
    productosTemplate = new ProductosTemplate(this);
}
```
***Nota:** Es importante que la obtención del servicio se posicione antes de la inyección con la parte gráfica ya que necesitamos tener lista la información antes de mostrar en pantalla*.

Vamos a crear un método que nos será útil una vez estemos en la clase template:

* Método para devolver un producto:
```javascript
public Producto devolverProducto(int posicion){
    return this.sProducto.devolverProducto(posicion);
}
```

Ahora nos concentraremos en la clase **ProductosTemplate**, primero vamos a realizar dos cosas importantes en las configuraciones generales del componente, vamos a cambiar el color de fondo por un gris claro y vamos a retirar la configuración que dejaba nulo el **LayoutManager**:
```javascript
this.setBackground(sRecursos.getColorGrisClaro());
**this.setLayout(null);*** // Se borra esta linea
```

Vamos a llamar a los servicios gráficos necesarios para ayudarnos con la creación de objetos gráficos:
* **Declaración:**
```javascript
private ObjGraficosService sObjGraficos;
private RecursosService sRecursos;
```

* **Obtención de servicios:**
```javascript
// Dentro del constructor
this.sRecursos = RecursosService.getService();
this.sObjGraficos = ObjGraficosService.getService();
```

Vamos a crear dos elementos que serán muy importantes para posicionar y colocar nuestros objetos gráficos:
* **Layout Manager tipo GridBadLayout**: Es el layout y quien se encargará de asignar el tamaño y posición y elementos dentro del componente.
* **Objeto de configuraciones GridBagConstraints**: Es el objeto encargado de configurar ciertas restricciones que nos ayudarán a indicarle al Layout como estará posicionado y que tamaño tendrá cada objeto gráfico.

* **Declaración:**
```javascript
private GridBagLayout lGrid;
private GridBagConstraints gbc;
```

* **Ejemplificación:**
```javascript
// Dentro del constructor
lGrid = new GridBagLayout();
gbc = new GridBagConstraints();
```

Antes de siquiera crear algún elemento en nuestro componente, conviene explicar de que se tratan cada una de estas restricciones que le indicaran al **GridBagLayout** como estarán posicionado nuestros elementos.

## Restricciones 

### **Posición de objeto dentro del Grid**:

Recordemos que un GridBagLayout crea una matriz imaginaria donde nosotros vamos a posicionar nuestros elementos de forma vertical u horizontal. Bien ps debemos siempre indicar en que fila y columna dentro del grid va comenzar algún elemento, esto lo haremos configurando el objeto **GridBagConstraints** y usando los atributos **gridx y gridy**.

La fila y columna inicial es la numero 0.

* **Ejemplo:**

Aquí estamos indicando que un elemento se posicionará en la fila 0 y columna 0 dentro del **GridBagLayout**.
```javascript
gbc.gridx = 0;
gbc.gridy = 0;
```

### **Espacio destinado**:

Como habíamos mencionado en la descripción de este **LayoutManager** a diferencia de un **GridLayout** en este Layout si tenemos la posibilidad de indicar si vamos a destinar mas de una celda en nuestra matriz o grid para un elemento en especifico. Para este caso podemos usar los atributos: 
* **gridwidth**: que nos indicará cuantas columnas van a estar destinadas para nuestro elemento dentro del Grid.
* **gridheight**: que nos indicará cuantas filas van a estar destinadas para nuestro elemento dentro del Grid.

Por defecto un elemento tendrá un **gridwidth y gridheight** de 1 es decir que ocupan una celda como espacio.

* **Ejemplo:**

Aquí nuestro elemento a posicionar tendrá destinado tres columnas y una fila dentro del Grid.
```javascript
gbc.gridwidth = 3;
gbc.gridheight = 1;
```

### **Estiramiento de objeto dentro de su espacio destinado**:

Independientemente de si un elemento tiene destinada una celda o pueda tener varias filas o columnas, puede que este objeto si requiera ocupar todo este espacio, o en caso contrario solo queremos que ocupe un espacio mínimo dentro de su espacio destinado y queremos intencionalmente que exista un campo sobrante (vació) dentro de su espacio destinado.

Otro caso podría ser un elemento el cual queremos que ocupe todo el ancho de su espacio pero el alto no lo queremos ocupar del todo, o viceversa.

Por defecto un elemento solo va a ocupar su tamaño justo, por ejemplo un botón con un texto solo ocupará lo suficiente para que el texto pueda leerse dentro del botón pero si queremos hacer que el botón se estire y ocupe todo su espacio podemos llamar al atributo **fill** y darle como valores:
* **NONE == 0:** Para que no se estire en ningún sentido, es la opción por defecto.
* **BOTH == 1:** Para que se estire en ambas dimensiones.
* **HORIZONTAL == 2:** Para que se estire sólo en horizontal.
* **VERTICAL == 3:** Para que se estire sólo en vertical.

Se puede usar el numero o la configuración explicita, como el desarrollador prefiera.

* **Ejemplo:**
```javascript
// Objeto sin estiramiento (por defecto)
gbc.fill = 0;
gbc.fill = GridBagConstraints.NONE;

// Objeto sin estiramiento de ambas dimensiones
gbc.fill = 1;
gbc.fill = GridBagConstraints.BOTH;

// Objeto sin estiramiento de forma horizontal
gbc.fill = 2;
gbc.fill = GridBagConstraints.HORIZONTAL;

// Objeto sin estiramiento de forma Vertical
gbc.fill = 3;
gbc.fill = GridBagConstraints.VERTICAL;
```

### **Posición dentro del espacio destinado:**

Para los objetos que no ocuparán todas las dimensiones de su espacio destinado, podemos configurar en que posición interna va a colocarse, para esto tenemos el atributo **anchor** y le podemos pasar como valor:
* **CENTER == 10:** Para que se posicione en el centro de la celda, Es la opción por defecto.
* **NORTH == 11:** Para que se posicione en a la parte superior y centrado en el eje horizontal.
* **NORTHEAST == 12:** Para que se posicione en la esquina superior derecha.
* **EAST == 13:** Para que se posicione en el lado derecho y centrado en el eje vertical.
* **SOUTHEAST == 14:** Para que se posicione en la esquina inferior derecha.
* **SOUTH == 15:** Para que se posicione en el lado inferior y centrado en el eje horizontal.
* **SOUTHWEST == 16:** Para que se posicione en la esquina inferior izquierda.
* **WEST == 17:** Para que se posicione en el lado izquierdo y centrado en el eje vertical.
* **NORTHWEST == 18:** Para que se posicione en la esquina superior izquierda.

Se puede usar el numero o la configuración explicita, como el desarrollador prefiera.

* **Ejemplo:**
```javascript
// Objeto gráfico posicionado en el centro
gbc.anchor = 10;
gbc.anchor = GridBagConstraints.CENTER;

// Objeto gráfico posicionado en la esquina superior derecha
gbc.anchor = 12;
gbc.anchor = GridBagConstraints.NORTHEAST;

// Objeto gráfico posicionado en el lado izquierdo
gbc.anchor = 17;
gbc.anchor = GridBagConstraints.WEST;
```

### **Margin de un objeto:**

El Margin de un objeto es el espacio que tendrá en pixeles con respecto al exterior, esto quiere decir el espacio entre el borde y los elementos que estén al rededor del objeto gráfico, puede ser hacia otro elemento que este arriba, abajo o a los lados, en caso de no haber algún elemento entonces será el espaciado contra la esquina de la ventana, panel u objeto que lo contenga. Para poder indicar cuanto espacio exterior tendrá el objeto actual en sus lados llamamos al atributo **insets** seguido de la opción:
* **top:** Margin hacia la parte superior.
* **right:** Margin hacia la parte derecha.
* **bottom:** Margin hacia la parte inferior.
* **left:** Margin hacia la parte izquierda.

Por defecto el margin es de 0px.
* **Ejemplo:**

En este ejemplo el elemento tiene un margin de 10px para arriba y abajo y un margin de 5px para la derecha e izquierda.
```javascript
gbc.insets.top = 10;
gbc.insets.right = 5;
gbc.insets.bottom = 10;
gbc.insets.left = 5;
```

### **Padding de un objeto:**
El Padding de un objeto es el espacio que tendrá en pixeles con respecto al interior, esto quiere decir cuanto espacio habrá entre el borde del objeto y los elementos internos de este. Es una buena forma para darle un poco mas de tamaño a nuestros elementos sin tener que estirar totalmente el objeto. Para poder indicar cuanto espacio interior tendrá el objeto actual en sus lados llamamos al atributo **ipadx** para el padding en el eje x o **ipady** para el padding en el eje y.

Por defecto el padding es de 0px.

**Ejemplo:**

En el siguiente ejemplo un elemento tiene un padding de 5px para el eje x y un padding de 10px para el eje y.
```javascript
gbc.ipadx = 5;
gbc.ipady = 10;
```
---

### **Estiramiento de filas o columnas**

Por defecto el grid que vamos a ir construyendo hará que todas las filas y columnas ocupen el ancho y alto de acuerdo al elemento que tenga mas altos valores en sus dimensiones, pero quizás pueden haber casos en los que queremos que una fila en especifico ocupe un ancho mayor al resto o quizás una columna para estos casos podemos usar los atributos: **weightx** para estirar una columna, o **weighty** para estirar una fila.

Por defecto el valor de estiramiento es de 0, para indicar que una fila será estirada se debe colocar el valor de 1. El estiramiento se realizara hasta lo permitido respecto a cuantos mas elementos habrá en el grid.

**Ejemplo:**

En este caso la primera columna se estirará con respecto a las demás, mientras que la primera fila estará con el alto por defecto.
```javascript
gbc.weightx = 1;
gbc.weighty = 0;
```

## Ajustando nuestra Ventana Principal

Como vamos a mostrar varios productos en nuestra ventana, es muy probable que estos ocupen mas dimensiones que tiene la ventana, es por esto que debemos realizar un método para poder navegar dentro de esta ventana, para eso vamos a utilizar un **JScrollPane** el cual nos brinda **JScrollBar** para que el usuario pueda navegar dentro de una ventana de dimensiones fijas como es nuestro caso. 

Vamos a ir a nuestra clase **VentanaPrincipalTemplate**, allí lo primero que vamos a hacer es declarar un objeto tipo **JScrollPane**:
```javascript
private JScrollPane psProductos;
```

Vamos a crear un método dentro de esta clase en la cual vamos a crear el JScrollPane, pero como vimos en la sesión de tablas, cuando creamos un JScrollPane debemos pasarle de inmediato el componente que este va a contener, es por eso que el método va a recibir como parámetro el panel que será contenido, en este caso el panel de productos:
```javascript
public void crearContenidoProductos(JPanel pProductos){

}
```
Vamos a construir nuestro ScrollPane pasando como componente al panel que recibimos por parámetro:
```javascript
public void crearContenidoProductos(JPanel pProductos){
    this.psProductos = sObjGraficos.construirPanelBarra(pProductos, 0, 0, 850, 600, null, null);
}
```

Vamos a personalizar el scroll vertical ya que será el único que necesitamos activar:
```javascript
public void crearContenidoProductos(JPanel pProductos){
    this.psProductos = sObjGraficos.construirPanelBarra(pProductos, 0, 0, 850, 600, null, null);
    this.psProductos.getVerticalScrollBar().setUI(sGraficosAvanzados.devolverScrollPersonalizado(
        7, 10, sRecursos.getColorGrisClaro(), sRecursos.getColorAzul(), sRecursos.getColorAzulOscuro())
    );
}
```

Ya tenemos listo el ScrollPane, ahora vamos a agregarlo en nuestro al panel principal de la ventana:

```javascript
public void crearContenidoProductos(JPanel pProductos){
    this.psProductos = sObjGraficos.construirPanelBarra(pProductos, 0, 0, 850, 600, null, null);
    this.psProductos.getVerticalScrollBar().setUI(sGraficosAvanzados.devolverScrollPersonalizado(
        7, 10, sRecursos.getColorGrisClaro(), sRecursos.getColorAzul(), sRecursos.getColorAzulOscuro())
    );
    this.pPrincipal.add(psProductos);
}
```

Finalmente y esto se hace para que el contenido del ScrollPane se vea sin problemas, muchas veces mostrar contenido dentro de un ScrollPane puede generar problemas de visualización asi que vamos a hacer una actualización de este, para eso vamos a mover el scroll automáticamente dos pixeles hacia abajo, y esto pasa ya que al activar el ScrollBar el sistema operativo obliga a nuestra, que pinte de nuevo el contenido y pueda ser visible.

```javascript
public void crearContenidoProductos(JPanel pProductos){
    this.psProductos = sObjGraficos.construirPanelBarra(pProductos, 0, 0, 850, 600, null, null);
    this.psProductos.getVerticalScrollBar().setUI(sGraficosAvanzados.devolverScrollPersonalizado(
        7, 10, sRecursos.getColorGrisClaro(), sRecursos.getColorAzul(), sRecursos.getColorAzulOscuro())
    );
    this.pPrincipal.add(psProductos);
    this.psProductos.getVerticalScrollBar().setValue(2);
}
```

Nuestro método esta listo, pero no lo hemos llamado, ¿Donde llamaremos a este método?, justo en el codigo que reacciona cuando el usuario oprime click en el botón de la navegación **Productos**, recordemos esa configuración esta dentro de la clase **VistaPrincipalComponent**, lo único que debemos hacer es cambiar la agregación directa del componente **Productos** al panel principal, por el paso al método que creamos:

<div align='center'>
    <img  src='https://i.imgur.com/YvSnaYJ.png'>
    <p></p>
</div>


## Creando nuestro componente padre

Siempre es bueno que antes de empezar a crear elementos y posicionarlos, hagamos un esquema general de como se van a distribuir los elementos dentro del GridBagLayout, a continuación mostramos el esquema de como se quiere construir el componente **Productos**:
<div align='center'>
    <img  src='https://i.imgur.com/mxWcfSB.png'>
    <p>Esquema de distribución dentro del Grid</p>
</div>

En este caso podemos ver las siguientes particularidades:
* La primera fila va a ocupar 3 columnas.
* Las demás filas estarán divididas en 3 columnas.
* No se aprecia en la imágen pero pueden haber muchas filas, dependiendo del numero de productos que coloquemos, mierras que el numero máximo de columnas va a ser siempre 3.

Vamos a crear nuestro primer elemento dentro de nuestro componente y a posicionarlo mediante las restricciones que acabamos de ver, en este caso queremos dar un titulo principal que nos indique que los productos mostrados serán cursos en linea, como será un único JLabel vamos a crearlo dentro del constructor:

* **Declaración:**
```javascript
private JLabel lTitulo;
```

* **Creación:**
```javascript
// Dentro del constructor
lTitulo = sObjGraficos.construirJLabel(
    "Cursos en Linea", 0, 0, 0, 0, null, sRecursos.getColorAzul(), null, sRecursos.getFontTProducto(), "c"
);
lTitulo.setBorder(sRecursos.getBorderInferiorAzul());
```

Noten que dentro de la creación de nuestro JLabel hemos dado una posición y tamaño de 0 ya que la posición por pixeles queda de lado y será el **GridBagLayout** se encargará de esto. Ahora, para configurar como se va a posicionar.

Ahora vamos a configurar las restricciones para posicionar nuestro elemento, no es necesario configurar todas las restricciones, solo las necesarias. Para empezar como nuestro titulo estará en la parte superior debemos indicar que nuestro titulo va a posicionarse en la columna y fila 0:
```javascript
// Dentro del constructor
lTitulo = sObjGraficos.construirJLabel(
    "Cursos en Linea", 0, 0, 0, 0, null, sRecursos.getColorAzul(), null, sRecursos.getFontTProducto(), "c"
);
lTitulo.setBorder(sRecursos.getBorderInferiorAzul());

gbc.gridx = 0;
gbc.gridy = 0;
```

Ahora vamos a indicar que queremos que nuestro label tenga un espacio exterior (Margin) de 15px para todos sus lados:
```javascript
// Dentro del constructor
lTitulo = sObjGraficos.construirJLabel(
    "Cursos en Linea", 0, 0, 0, 0, null, sRecursos.getColorAzul(), null, sRecursos.getFontTProducto(), "c"
);
lTitulo.setBorder(sRecursos.getBorderInferiorAzul());

gbc.gridx = 0;
gbc.gridy = 0;
gbc.insets.top = 15;
gbc.insets.bottom = 15;
gbc.insets.left = 15;
gbc.insets.right = 15;
```

Ahora como en esquema podemos darnos cuenta, la primera fila va a ocupar 3 columnas asi que debemos indicar esto a nuestras restricciones:
```javascript
// Dentro del constructor
lTitulo = sObjGraficos.construirJLabel(
    "Cursos en Linea", 0, 0, 0, 0, null, sRecursos.getColorAzul(), null, sRecursos.getFontTProducto(), "c"
);
lTitulo.setBorder(sRecursos.getBorderInferiorAzul());

gbc.gridx = 0;
gbc.gridy = 0;
gbc.insets.top = 15;
gbc.insets.bottom = 15;
gbc.insets.left = 15;
gbc.insets.right = 15;
gbc.gridwidth = 3;
```

Por ultimo, queremos que nuestro label ocupe todo el ancho horizontal de las 3 columnas asi que vamos a estirar el objeto en este sentido:

```javascript
// Dentro del constructor
lTitulo = sObjGraficos.construirJLabel(
    "Cursos en Linea", 0, 0, 0, 0, null, sRecursos.getColorAzul(), null, sRecursos.getFontTProducto(), "c"
);
lTitulo.setBorder(sRecursos.getBorderInferiorAzul());

gbc.gridx = 0;
gbc.gridy = 0;
gbc.insets.top = 15;
gbc.insets.bottom = 15;
gbc.insets.left = 15;
gbc.insets.right = 15;
gbc.gridwidth = 3;
gbc.fill = GridBagConstraints.HORIZONTAL;
```
Ya terminamos con todas las restricciones, ahora le debemos indicar al **GridBagLayout** que tendrá las restricciones actuales ese elemento, esto con el método **setConstraints** que recibe como parámetros:
* **El objeto gráfico que se añadirá**.
* **El objeto de configuración de restricciones.**
```javascript
// restricciones
...
...

lGrid.setConstraints(lTitulo, gbc);
```

Ahora simplemente agregamos el elemento al componente:
```javascript
// restricciones
...
...

lGrid.setConstraints(lTitulo, gbc);
this.add(lTitulo);
```

***Nota1:** Muy seguramente si ejecutamos la aplicación se verá nuestro titulo en la mitad y sin las propiedades que hemos indicado, esto eso por que es el único elemento por el momento y en estos casos el GridBagLayout coloca esos únicos elementos en el medio.*

<div align='center'>
    <img  src='https://i.imgur.com/wMMtYV9.png'>
    <p></p>
</div>

***Nota2:** Debemos tener cuidado con las restricciones que vamos configurando ya que estas quedan así a menos que se vuelan a cambiar, por ejemplo ya indicamos que el titulo ocupará 3 columnas, pero esto no será asi para los demás elementos que coloquemos así que debemos modificarlo de nuevo para cambiar su valor. Debemos tener cuidado de volver a configurar las restricciones o puede que por alguna de estas que no se haya actualizado todo nuestro esquema quede mal.*

Es hora de llamar a nuestros productos, para esto vamos a crear un método llamado **crearProductos**:
```javascript
public void crearProductos(){
}
```

Lo primero que haremos es declarar dos variables enteras que nos ayudaran para el posicionamiento de nuestros productos y ademas para obtenerlos:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
}
```

Ahora vamos a empezar a llamar los productos, en este caso se llamará al primero a traves del método de la clase **Component** que ya configuramos:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
}
```

El paso siguiente es muy importante y es que como indicamos hay restricciones que ya se configuraron para colocar el titulo pero alguna de estas no las queremos asi que debemos volver a colocarlas en su valor por defecto, en este caso para cada producto no queremos:
* Que cada panel de un producto se estire en el eje horizontal.
* Que cada producto ocupe 3 columnas, solo queremos que ocupe 1.
* Que tenga un margin a la derecha de 15, si queremos un margin de 15 para los demás lados pero para la derecha no será necesario.
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
}
```

Crearemos un ciclo while y la condición para que este continue es que el objeto de producto no sea nulo, o en otras palabras el ciclo se mantendrá hasta que acabemos de recorrer la lista de productos:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
    }
}
```

Dentro del While vamos a realizar **Reutilización de componentes con enfoque de posicionamiento**, es decir que dentro del while vamos a llamar al componente hijo **Producto**, recordemos que debemos llamar a su parte lógica para poder realizar una comunicación pero necesitamos obtener su parte gráfica:

```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
        ProductoTemplate pProducto = new ProductoComponent().getProductoTemplate();
    }
}
```

Ahora para que el componente hijo **Producto** pueda mostrar la información de cada curso debemos pasarle como argumento los datos de cada curso o para no mandar tantos argumentos podemos directamente pasarle el objeto del producto:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
        ProductoTemplate pProducto = new ProductoComponent(producto).getProductoTemplate();
    }
}
```

Esto nos sacará error por que en nuestro componente hijo aun no pedimos nada por parámetro pero es lo que haremos a continuación:
* **Clase ProductoComponent:**
```javascript
public ProductoComponent(Producto producto){
    productoTemplate = new ProductoTemplate(this, producto);
}
```

* **Clase ProductoTemplate:**
```javascript
public ProductoTemplate(ProductoComponent productoComponent, Producto producto){
    ...
    ...
}
```

Dentro de la case **ProductoTemplate** vamos a igualar el parámetro recibido con un atributo que declaremos global ya que es probable que lo necesitemos en otras partes distintas al constructor:
```javascript
private Producto producto;

public ProductoTemplate(ProductoComponent productoComponent, Producto producto){
    this.producto = producto;
    ...
    ...
}
```

Por ahora la parte template del componente hijo no tiene nada, esto se configurará en breve, vamos a seguir con el componente padre. Una vez obtenido el objeto gráfico del componete hijo debemos empezar a configurar su posicion, debemos indicar en que fila y columna se va a posicionar cada uno de los productos:
* Para las columnas nos ayudaremos de la operación modulo con 3 y el numero del producto que tenemos actualmente, asi:
    * si estamos con el producto numero 0 el modulo con 3 dará 0.
    * si estamos con el producto numero 1 el modulo con 3 dará 1.
    * si estamos con el producto numero 2 el modulo con 3  dará 2.
    * si estamos con el producto numero 3 el modulo con 3  dará 0.

Así podemos darnos cuenta que gracias al modulo el valor de las columnas va a oscilar entre 0 a 2 que es justamente lo que queremos.
* Para las filas vamos a ayudarnos con la variable **fila** y pueden notar que la inicializamos con 1 por que recordemos que la primera fila ya esta ocupada por el título.
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
        ProductoTemplate pProducto = new ProductoComponent(producto).getProductoTemplate();
        gbc.gridx = numProducto % 3;
        gbc.gridy = fila;
    }
}
```

Estas son todas las restricciones que configuraremos asi que ahora debemos añadir las configuraciones actualizadas y añadir nuestro producto:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
        ProductoTemplate pProducto = new ProductoComponent(producto).getProductoTemplate();
        gbc.gridx = numProducto % 3;
        gbc.gridy = fila;
        lGrid.setConstraints(pProducto, gbc);
        this.add(pProducto);
    }
}
```

Ahora debemos actualizar los valores de cada iteración para que el ciclo recorra todos los productos y no quede intermitente, primero debemos actualizar el valor de la fila y es que si ya hay 3 productos ocupando la fila es hora de sumarle el valor a la fila:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
        ProductoTemplate pProducto = new ProductoComponent(producto).getProductoTemplate();
        gbc.gridx = numProducto % 3;
        gbc.gridy = fila;
        lGrid.setConstraints(pProducto, gbc);
        this.add(pProducto);
        if(numProducto % 3 == 2)
            fila ++;
    }
}
```

Ahora debemos actualizar el numero del producto y asi mismo debemos llamar al siguiente producto en la lista:
```javascript
public void crearProductos(){
    int numProducto = 0, fila = 1;
    Producto producto = productosComponent.devolverProducto(numProducto);
    gbc.fill = GridBagConstraints.NONE;
    gbc.gridwidth = 1;
    gbc.insets.right = 0;
    while(producto != null){
        ProductoTemplate pProducto = new ProductoComponent(producto).getProductoTemplate();
        gbc.gridx = numProducto % 3;
        gbc.gridy = fila;
        lGrid.setConstraints(pProducto, gbc);
        this.add(pProducto);
        if(numProducto % 3 == 2)
            fila ++;
        numProducto ++;
        producto = productosComponent.devolverProducto(numProducto);
    }
}
```

Finalmente y para que mas adelante podamos ver la estructura, debemos llamar el método dentro del constructor:
```javascript
this.crearProductos();
```

En teoría ya tenemos configurada la estructura básica de nuestro componente padre, pero como nuestro componente hijo aun no tiene un contenido, no se notará aun esta estructura, en la siguiente sección vamos a crear el contenido de cada producto para poder ver por fin como quedá nuestro componente gráfico:


## Creando nuestro componente hijo

Dentro de la clase **ProductoTemplate** vamos a crear la estructura básica de un producto y como dijimos antes es bueno realizar un esquema general de como queremos que se vea un producto:
<div align='center'>
    <img  src='https://i.imgur.com/O7NmZ6L.png'>
    <p>Esquema general de un producto</p>
</div>

vamos a dar las características generales al componente: 
* Primero vamos a darle un color de fondo blanco
* Vamos a darle un borde redondeado a nuestro producto
* Vamos a decirle que sea visible

```javascript
// Dentro del constructor
this.setBackground(Color.WHITE);
this.setBorder(sRecursos.getBordeRedondeado());
this.setVisible(true);
```

Ahora queremos que cada producto tenga en este caso un tamaño fijo, para eso vamos a dar una dimensión fija a nuestro componente:

```javascript
// Dentro del constructor
this.setPreferredSize(new Dimension(262, 330));
```

Ahora podemos ver que la estructura del componente padre **Productos** funciona:

<div align='center'>
    <img  src='https://i.imgur.com/rMnQ7c6.png'>
    <p>Estructura del componente padre</p>
</div>

Vamos a declarar nuestros objetos que estarán a cargo del posicionamiento y tamaño de los objetos gráficos que serán contenidos:

* **Declaración:**
```javascript
// Declaración LayoutManager
private GridBagLayout lGrid;
private GridBagConstraints gbc;
```

* **Ejemplificación:**
```javascript
lGrid = new GridBagLayout();
gbc = new GridBagConstraints();
```

Ahora podemos indicarle al componente que su LayoutManager será este **GridBagLayout**.

```javascript
lGrid = new GridBagLayout();
gbc = new GridBagConstraints();
this.setLayout(lGrid);
```

A continuación se declaran los objetos gráficos y decoradores que vamos a crear dentro del componente gráfico:
```javascript
// Declaración Objetos Gráficos
private JLabel lTitulo, lImagen, lParrafo, lEstrella, lPuntuacion, lCampo;

// Declaración Objetos Decoradores
private ImageIcon iEstrella, iDimAux;
```

Podemos notar que en este caso serán todos JLabels, pero serán varios asi que nos conviene crear un método para la creación de estos:
```javascript
public void crearJLabels(){
}
```

Ahora como sera una cantidad considerable de labels los que vamos a crear, es probable que modificar algunas restricciones y luego tener encuentra cuales deben ser modificadas para el siguiente objeto será una tarea tediosa y ademas puede que ocupe bastantes lineas de código, podría ser una mejor solución si creamos un método que se encargue de la actualización de cada una de las restricciones por objeto y de esta forma no solo nos ahorramos lineas de código, también nos aseguramos de actualizar cada restricción y asi no tener problemas de posicionamiento en el futuro:

```javascript
public void modificarGbc(
    int posColumna, int posFila, int numColumnas, int numFilas, int tipoEstirado, int posicionInterna, 
    int marginArriba, int marginDerecha, int marginAbajo, int marginIzquierda, int paddingX, int paddingY,
    int estiramientoColumna, int estiramientoFila
){
    gbc.gridx = posColumna;
    gbc.gridy = posFila;
    gbc.gridwidth = numColumnas;
    gbc.gridheight = numFilas;
    gbc.fill = tipoEstirado;
    gbc.anchor = posicionInterna;
    gbc.insets.top = marginArriba;
    gbc.insets.right = marginDerecha;
    gbc.insets.bottom = marginAbajo;
    gbc.insets.left = marginIzquierda;
    gbc.ipadx = paddingX;
    gbc.ipady = paddingY;
    gbc.weightx = estiramientoColumna;
    gbc.weighty = estiramientoFila;
}
```

Dentro de este método están contenidas todas las restricciones que puedan tener, nuestro objeto **GridBagConstraints** y aunque no en todos los casos vamos a necesitar configurar todas las restricciones las podemos dejar con su valor por defecto y ademas nos vamos a ahorrar muchas lineas de código.

Dentro del método **crearLabels** vamos a construir nuestro primer Label que será el titulo o nombre del producto:
```javascript
public void crearJLabels(){
    lTitulo = sObjGraficos.construirJLabel(
        producto.getNombreProducto(), 0, 0, 0, 0, null, sRecursos.getColorAzul(),
        null, sRecursos.getFontTitulo(), "l"
    );
}
```

Ahora vamos a configurar todas sus restricciones llamando al método que creamos para esto y explicaremos que representan cada uno de estos números, ademas vamos a agregarlo de una vez al componente:
```javascript
public void crearJLabels(){
    lTitulo = sObjGraficos.construirJLabel(
        producto.getNombreProducto(), 0, 0, 0, 0, null, sRecursos.getColorAzul(),
        null, sRecursos.getFontTitulo(), "l"
    );
    modificarGbc(0, 0, 3, 1, 0, 10, 10, 0, 0, 0, 0, 0, 0, 0);
    this.add(lTitulo, gbc);
}
```

***Nota:** Noten que podemos agregar un elemento directamente con sus respectivas configuraciones **GridBagConstraints** pero para que esto funcione nuestro componente debe ya tener configurado el **LayoutManager**, esto quiere decir que la linea de código **setLayoutManager(lGrid)**, debe estar antes de llamar este método (crearLabels) o de lo contrario no funcionara, en cambio cuando usamos el otro enfoque (como lo hicimos en la clase **ProductosTemplate**) no es necesario que la configuración del layout esté antes de la configuración de las posiciones de los elementos, esa es la diferencia de por que en la clase **ProductosTemplate** esta al final mientras que en la clase **ProductoTemplate** esta al comienzo.*

Ahora para el caso del titulo estamos diciendo:
* Va a posicionarse en la columna y fila 0 (la primera en cada caso).
* Va a ocupar 3 columnas y 1 fila.
* No se va a estirar el objeto (se deja valor en defecto 0).
* Se quiere que el titulo quede en el medio (se deja valor por defecto 10).
* Se deja un margin de 10px con el espacio de arriba, con los otros lados serán de 0px.
* Se deja un padding de 0px para ambas dimensiones.
* No se quiere estirar la fila ni la columna (se deja en 0).

Vamos ahora con nuestro siguiente Label que en este caso será la imágen del producto:
```javascript
// Dentro del método crearLabels
iDimAux = new ImageIcon(producto.getImagen().getImage().getScaledInstance(250, 148, Image.SCALE_AREA_AVERAGING));

lImagen = sObjGraficos.construirJLabel(null, 0, 0, 0, 0, iDimAux, null, null, null, "c");
lImagen.setBorder(sRecursos.getBordeRedondeado());
modificarGbc(0, 1, 3, 1, 2, 10, 10, 3, 10, 3, 0, 0, 0, 0);
this.add(lImagen, gbc);
```

Para este caso debemos tener en cuenta que la resolución de la imágen y esta es la que dará el tamaño al label, en cuanto a sus restricciones tenemos:
* Va a posicionarse en la columna 0 y en la fila 1.
* Va a ocupar 3 columnas y 1 fila.
* Se va a estirar la imagen de forma horizontal asi que se deja un numero 2.
* Se quiere que la imágen quede en el medio (se deja valor por defecto 10).
* Se deja un margin de 10px con el espacio de arriba y abajo, con los lados laterales serán de 3px.
* Se deja un padding de 0px para ambas dimensiones.
* No se quiere estirar la fila ni la columna (se deja en 0).

El siguiente elemento será el párrafo de descripción del producto:

```javascript
// Dentro del método crearLabels
lParrafo = sObjGraficos.construirJLabel(
    "<html><div align='justify'>" + producto.getDescripcion() + "</div></html>", 0, 0, 0, 0, 
    null, sRecursos.getColorGrisOscuro(), null, sRecursos.getFontPequeña(), "l"
);
modificarGbc(0, 2, 3, 1, 2, 10, 10, 15, 10, 15, 0, 0, 0, 0);
this.add(lParrafo, gbc);
```

Para este caso en cuanto a sus restricciones tenemos:
* Va a posicionarse en la columna 0 y en la fila 2.
* Va a ocupar 3 columnas y 1 fila.
* Se va a estirar el párrafo de forma horizontal asi que se deja un numero 2.
* Se quiere que la imágen quede en el medio (se deja valor por defecto 10).
* Se deja un margin de 10px con el espacio de arriba y abajo, con los lados laterales serán de 15px.
* Se deja un padding de 0px para ambas dimensiones.
* No se quiere estirar la fila ni la columna (se deja en 0).

Ahora vamos a crear el label del campo o area del curso:
```javascript
// Dentro del método crearLabels
lCampo = sObjGraficos.construirJLabel(
    producto.getCampo(), 0, 0, 0, 0, null, sRecursos.getColorAzulMarino(), sRecursos.getColorAzulClaro(), sRecursos.getFontBotones(), "c"
);
modificarGbc(0, 3, 1, 1, 0, 10, 10, 5, 15, 5, 10, 10, 0, 0);
this.add(lCampo, gbc);
```

Para este caso en cuanto a sus restricciones tenemos:
* Va a posicionarse en la columna 0 y en la fila 3.
* Va a ocupar 1 columna y 1 fila.
* No se quiere estirar el objeto (se deja valor por defecto 0).
* Se quiere que la imágen quede en el medio (se deja valor por defecto 10).
* Se deja un margin de 10px con el espacio de arriba, 15px con respecto al espacio de abajo, con los lados laterales serán de 5px.
* Se deja un padding de 10px para ambas dimensiones.
* No se quiere estirar la fila ni la columna (se deja en 0).

Ahora vamos a crear el label que representa la imagén de una estrella:
```javascript
// Dentro del método crearLabels
iEstrella = new ImageIcon("Clase15/resources/img/estrella.png");
iDimAux = new ImageIcon(iEstrella.getImage().getScaledInstance(25, 25, Image.SCALE_AREA_AVERAGING));

lEstrella = sObjGraficos.construirJLabel(null, 0, 0, 0, 0, iDimAux, null, null, null, "c");
modificarGbc(1, 3, 1, 1, 0, 13, 10, 5, 15, 5, 0, 0, 1, 0);
this.add(lEstrella, gbc);
```

Para este caso ejemplificamos primero la imágen para obtenerla y luego si la redimensionamos, en cuanto a las restricciones tenemos:

* Va a posicionarse en la columna 1 y en la fila 3.
* Va a ocupar 1 columna y 1 fila.
* No se quiere estirar el objeto (se deja valor por defecto 0).
* Se quiere pegar la imagen con el lado derecho de su espacio destinado asi que se deja el valor de 13.
* Se deja un margin de 10px con el espacio de arriba, 15px con respecto al espacio de abajo, con los lados laterales se dejan 5px.
* Se deja un padding de 0px para ambas dimensiones.
* En este caso se quiere estirar la columna del medio es decir la columna que ocupa este objeto asi que se deja en 1, para el caso de la fila si se deja normal.

Finalmente vamos a agregar el puntaje del curso:
```javascript
// Dentro del método crearLabels
lPuntuacion = sObjGraficos.construirJLabel(
    producto.getPuntuacion() + "/ 5", 0, 0, 0, 0, null, sRecursos.getColorAzulMarino(), 
    null, sRecursos.getFontBotones(), "l"
);
modificarGbc(2, 3, 1, 1, 0, 16, 10, 10, 15, 0, 0, 0, 0, 0);
this.add(lPuntuacion, gbc);
```

Para este caso en cuanto a sus restricciones tenemos:
* Va a posicionarse en la columna 2 y en la fila 3.
* Va a ocupar 1 columna y 1 fila.
* No se quiere estirar el objeto (se deja valor por defecto 0).
* Se quiere que el label quede posicionado en la parte inferior izquierda, para que quede pegado a la izquierda y este a la misma altura con la imágen de la estrella o de lo contrario se vera un poco mas arriba de ella.
* Se deja un margin de 10px con el espacio de arriba y a la derecha, 15px con respecto al espacio de abajo y 0px para la izquierda.
* Se deja un padding de 10px para ambas dimensiones.
* No se quiere estirar la fila ni la columna (se deja en 0).

Ya esta listo nuestro método, ahora debemos llamar nuestro método en el constructor, recordemos que debemos llamar al método después de configurar el LayoutManager al componente:
```javascript
// Dentro del constructor
lGrid = new GridBagLayout();
gbc = new GridBagConstraints();
this.setLayout(lGrid);

this.crearJLabels();
...
```

Finalmente nuestra interfaz gráfica queda así:
<div align='center'>
    <img  src='./gifs/final.gif'>
    <p>Resultado final del uso de GridBagLayout</p>
</div>

# Resultado 

Si llegaste hasta aquí **!felicidades!** aprendiste mucho sobre **LayoutManager** y entre muchas cosas aprendiste **Las características generales de los LayoutManager y que ventajas tiene frente a otros enfoques**, **Los tipos de LayoutManager, como y cuando usar cada tipo**, **Aprendiste a usar el GridBagLayout** que es quizás el mas sofisticado y mostramos finalmente una lista de productos.

# Actividades 

Utiliza **GridBagLayout** para posicionar elementos dentro de tu proyecto.