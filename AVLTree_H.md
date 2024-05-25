```c++
struct Node {
    int value;
    Node* left;
    Node* right;
    int height;
};
```
**Se define la estructura Node que contiene los miembros:** 
- `int value`: es el valor que contendrá el nodo.
- `Node* left`: el puntero al nodo izquierdo.
- `Node* right`: el puntero al nodo derecho.
- `int height`: la altura de este nodo.
```c++
class AVLTree {
```
Clase que representa el AVLTree
```c++
public:
    void printTree() {
        printTree(root, 0, 10);
    }
```
Atributos públicos de la clase AVLTree:
- `void printTree()`: Método para imprimir todo el árbol de manera visual. 
```c++

private:
    Node* root;

    int getNodeHeight(Node* N) {
        if (N == nullptr)
            return 0;
        return N->height;
    }

    int maxHeight(int a, int b) {
        return (a > b)? a : b;
    }
    
```
Primeros 2 atributos privados de la clase AVL:
- `Node* root`: Puntero a la raíz del árbol AVL. De esta se deriva todo el árbol.
- `int getNodeHeight(Node* N)`: Método que toma un puntero al nodo, y devuelve su altura(int). Si el nodo es nulo, la altura será 0. 
- `int maxHeight(int a, int b)`: Método que devuelve la mayor de dos alturas `a` y `b`, para comparar las alturas de los nodos y determinar la altura del nodo padre basado en la altura de sus hijos.
Si `a` es mayor a `b` devuelve `a`, de lo contrario devolverá `b`.

Continuando con los atributos privados...
```c++
Node* newNode(int value) {
    Node* node = new Node();
    node->value = value;
    node->left = nullptr;
    node->right = nullptr;
    node->height = 1;
    return(node);
}
```
Este método devuelve un puntero al nuevo nodo. Este nuevo nodo recibe como parámetro
el valor que contendrá en nuevo nodo del árbol AVL.
A este se le inicializarán sus miembros de la siguiente manera: 
- `Node* node = new Node()`: Aquí se crea un nuevo objeto `Node` en la memoria dinámica(new). El objeto es un Nodo del árbol AVL.
- `node->value = value`: Se le asigna el valor pasado por parámetro al nodo nuevo.
- `node->left = nullptr` y `node->right = nullptr`: Como este nodo no tiene hijos AÚN, se inicializan como nulos, tanto el izquierdo como el derecho.
- `node->height = 1`: Se inicializa la altura del nodo con valor de 1. Como aún no hay nodos hijos, la altura de este será 1 al principio y
posteriormente cuando insertemos nuevos nodos hijos, la altura de este aumentará.
- `return(node)`: Finalmente retorna el nodo creado.

```c++
    Node* rightRotate(Node* y) {
        Node* x = y->left;
        Node* T2 = x->right;
        
        x->right = y;
        y->left = T2;
        
        y->height = maxHeight(getNodeHeight(y->left), getNodeHeight(y->right)) + 1;
        x->height = maxHeight(getNodeHeight(x->left), getNodeHeight(x->right)) + 1;
        
        return x;
    }
```
Este método realiza una rotación a la derecha en caso de que haya un desequilibrio a la izquierda.
Esto pasa cuando en el subarbol izquierdo hay más profundidad que en el derecho, lo cual hace que el árbol pierda sus 
propiedades de equilibrio.
- `Node* rightRotate(Node* y)`: Recibe como parámetro el puntero a un Nodo `y` que es el nodo que raíz a rotar.
- `Node* x = y->left`: En nodo `x` apuntará al hijo izquierdo de el nodo raíz `y`.
- `Node* T2 = x->right`: El nodo `T2` será el hijo derecho de `x`.
- `x->right = y`: Como se rota a la derecha, el nuevo nodo raíz será x, que tendrá como hijo izquierdo a y.
- `y->left = T2`: Finalmente el nodo `T2` será el hijo derecho de y, completando la rotación.
- `y->height` y `x->height`: Modificará la altura de cada nodo.
Usando la función `maxHeight` para calcular la altura, pasando como parámetro la función `getNodeHeight`
que accederá al nodo que esté a la izquierda de este y como segundo parámetro la altura del nodo derecho.
Cuando obtenga la mayor altura entre esas dos alturas, le sumará 1.

```c++
    Node* leftRotate(Node* x) {
        Node* y = x->right;
        Node* T2 = y->left;
        
        y->left = x;
        x->right = T2;
        
        x->height = maxHeight(getNodeHeight(x->left), getNodeHeight(x->right)) + 1;
        y->height = maxHeight(getNodeHeight(y->left), getNodeHeight(y->right)) + 1;
        
        return y;
}
```
Este método es similar al anterior, pero esta vez es para rotar a la izquierda en caso de que el subárbol de la derecha
esté desequilibrado. Recibe como parámetro el nodo raíz del subárbol desequilibrado.
- `Node* y = x->right`: Este puntero de un nodo `y` el cual accede al hijo derecho de `x`.
- `Node* T2 = y->left`: El puntero T2 será el hijo izquierdo de `y`.
- `y->left = x`: Aquí se moverá el nodo `x` para ser hijo izquierdo de `y`, dejando a `y` como el nuevo nodo raíz 
del árbol AVL.
- `x->right = T2`: Luego el subárbol T2 será hijo derecho de `x`, esto para evitar datos duplicados o perdidos.
- `x->height` y `y->height`: Se calculan las nuevas alturas de los nodos después del balance.
Se usa la función `maxHeight` con los parámetros `getNodeHeight` que toma las alturas de los hijos
izquierdo y derecho tanto de `x` como de `y`, se comparan las dos alturas de los hijos y a la mayor altura se le sumará 1, esto
dará la nueva altura del nodo después del reubicado o balanceo.

```c++
  int getBalance(Node* N) {
  if (N == nullptr)
  return 0;
  return getNodeHeight(N->left) - getNodeHeight(N->right);
  }
```
El método `getBalance` devuelve un entero, el cual indica el balance de un nodo del
árbol AVL. Si el nodo pasado por parámetro es nulo se devolverá 0, de lo contrario 
devolverá la diferencia entre la altura del hijo izquierdo con la del hijo derecho, esto
indicará si el árbol está equilibrado o desbalanceado, y realizar una rotación derecha o izquierda.

Node* insertNode(Node* node, int value) {
if (node == nullptr)
return(newNode(value));

```c++
        Node* insertNode(Node* node, int value) {
            if (node == nullptr)
                return(newNode(value));
            if (value < node->value)
                node->left = insertNode(node->left, value);
            else if (value > node->value)
                node->right = insertNode(node->right, value);
            else
                return node;

            node->height = 1 + maxHeight(getNodeHeight(node->left), getNodeHeight(node->right));

            int balance = getBalance(node);

            if (balance > 1 && value < node->left->value)
                return rightRotate(node);

            if (balance < -1 && value > node->right->value)
                return leftRotate(node);

            if (balance > 1 && value > node->left->value) {
                node->left = leftRotate(node->left);
                return rightRotate(node);
            }

            if (balance < -1 && value < node->right->value) {
                node->right = rightRotate(node->right);
                return leftRotate(node);
            }

            return node;
    }
```
El método `insertNode` se encarga de insertar un nuevo nodo en el árbol AVL en la 
posición correcta de manera balanceada.
El método tiene los siguientes parámetros: 
- `Node* node`: El nodo raíz del subárbol donde se inserta el nuevo valor.
- `int value`: El valor a insertar en el árbol.
El método funciona con las siguientes condicionales:
- `if (node == nullptr)`: si el nodo es nulo o vacío se crea un nuevo nodo con el valor proporcionado 
en su interior usando `newNode(value)`.
- `if (value < node->value)` En caso de que el valor proporcionado sea menor que el valor del nodo actual
se realiza recusión insertando ese nodo nuevo como hijo izquierdo del nodo actual.
-  `else if (value > node->value)`: En caso distinto, cuando el valor porporcionado sea mayor
que el valor del nodo actual, se realiza recursión para para colocar ese valor como hijo derecho del nodo.
- `else return node`: Si el valor es igual al valor del nodo actual, se devuelve el nodo, ya que no se 
puede colocar valores duplicados en un árbol binario de búsqueda.
- `node->height`: calcula la nueva altura del nodo comparando entre la altura del nodo izquierdo y el nodo derecho y
tomando el mayor de estos, para sumarle 1 y asignarle esa nueva altura al nodo.
- `int balance`: Con la función `getBalance(node)` toma el balance del nodo.
- `if (balance > 1 && value < node->left->value)`: Si el balance es mayor a 1 significa que hay un desbalance hacia 
la izquierda. Dependiendo de si el nuevo valor es mayor o menor que el valor del subárbol izquierdo se realiza una rotación derecha,
o una combinación de rotaciones izquierda-derecha (`leftRotate` seguido de `rightRotate`).4
-  `if (balance < -1 && value > node->right->value)`: Si el balance es menor a `-1` significa que hay
un desbalance a la derecha. Dependiendo de la relación entre el valor nuevo y el valor
del subárbol derecho se realizará una rotación a la izquierda o una combinación derecha-izquierda (`rightRotate` seguido de `leftRotate`).
- `if (balance > 1 && value > node->left->value)`:En caso de que el balance sea mayor a `1` significa
que hay un desbalance a la izquierda. Si el valor que se está insertando es mayor que el 
valor del subárbol izquierdo se realizará una rotación izquierda al hijo izquierdo del nodo actual,
pero menor que el nodo actual, lo que crea una situación de subárbol izquierda-derecha.
Para corregir el desbalanceo se realiza una rotación a la izquierda en el subárbol izquierdo
  `(node->left = leftRotate(node->left))`. Esto convierte el subárbol izquierda-derecha en un subárbol izquierda-izquierda.
- `if (balance < -1 && value < node->right->value)`: Si hay un desbalance a la derecha (balance menor a `-1`) y 
el valor que se está insertando es menor que el valor del subárbol derecho, pero mayor que el nodo actual 
`(creando un subárbol derecha-izquierda)`, se aplica una rotación a la derecha en el subárbol derecho (node->right = rightRotate(node->right))
Esto convierte el subárbol derecha-izquierda en un subárbol derecha-derecha.
Finalmente retornamos al nodo con sus modificaciones realizadas.

```c++
    Node* minValueNode(Node* node) {
        Node* current = node;

        while (current->left != nullptr)
            current = current->left;

        return current;
    }
```
Este método devolverá el nodo más profundo desde un nodo inicial.
Toma un puntero a un nodo como parámetro, y luego se asigna a
un puntero de un objeto tipo Node `(Node* current = node)`
- `while (current->left != nullptr)`: Mientras el hijo izquierdo del nodo actual `current`
no esté vacío, se moverá a los hijos izquierdos hasta que el siguiente sea nulo.
Finalmente retorna el puntero al nodo más profundo al que llegó en el loop `while`.
```c++
      Node* deleteNode(Node* root, int value) {
        if (root == nullptr)
        return root;

        if ( value < root->value )
            root->left = deleteNode(root->left, value);
        else if( value > root->value )
            root->right = deleteNode(root->right, value);
        else {
            if( (root->left == nullptr) || (root->right == nullptr) ) {
                Node *temp = root->left ? root->left : root->right;

                if(temp == nullptr) {
                    temp = root;
                    root = nullptr;
                }
                else
                    *root = *temp;

                delete temp;
            }
            else {
                Node* temp = minValueNode(root->right);

                root->value = temp->value;

                root->right = deleteNode(root->right, temp->value);
            }
        }

        if (root == nullptr)
            return root;

        root->height = 1 + maxHeight(getNodeHeight(root->left), getNodeHeight(root->right));

        int balance = getBalance(root);

        if (balance > 1 && getBalance(root->left) >= 0)
            return rightRotate(root);

        if (balance > 1 && getBalance(root->left) < 0) {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }

        if (balance < -1 && getBalance(root->right) <= 0)
            return leftRotate(root);

        if (balance < -1 && getBalance(root->right) > 0) {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }

        return root;
  }
```       
El método deleteNode elimina un nodo con un valor específico de un árbol AVL,
asegurando que el árbol permanezca equilibrado después de la eliminación. 

- `if (root == nullptr)`: Si root es nullptr, el árbol está vacío y no hay nada que eliminar, por lo que se devuelve root.
- `if ( value < root->value)`: Si el value es menor que root->value, el nodo a eliminar está en el subárbol izquierdo, por lo que se llama recursivamente deleteNode(root->left, value).
- `else if( value > root->value)`: Si el valor es mayor al valor del nodo actual, el nodo a eliminar está en el subárbol derecho, por lo que se llama recursivamente deleteNode(root->right, value).
- Si `value` es igual a `root->value`, se ha encontrado el nodo a eliminar.
- `if( (root->left == nullptr) || (root->right == nullptr) )`: Condicional que se cumple cuando el nodo solo tiene un hijo.
En la variable `temp` se guarda el único hijo que tiene el nodo, si no tiene hijos este será nulo.
- `if(temp == nullptr)`: En caso de que `temp` sea nulo (no tiene hijos), temp será la raíz y posteriormente se eliminará el nodo.
- Si temp si tiene hijos, `root` se igualará a ese hijo como punteros y posteriormente se eliminará `temp` para liberar la memoria.
```c++
    else {
        Node* temp = minValueNode(root->right);

        root->value = temp->value;

        root->right = deleteNode(root->right, temp->value);
    }
```
Si el nodo tiene 2 hijos:
- `Node* temp = minValueNode(root->right)`: Se busca el valor mínimo en el subárbol derecho. 
- `root->value = temp->value`: El valor del nodo `root` ahora valdrá `temp`, completando la eliminación.
- `root->right = deleteNode(root->right, temp->value)`: Se elimina el sucesor del subárbol derecho llamando recursivamente a `deleteNode(root->right, temp->value)`
  para evitar duplicación y mantener la estructura del árbol.
```c++
    if (root == nullptr)
        return root;
```
Si el nodo es nulo se retornará ese nodo para indicar que no hay nada que eliminar.
```c++ 
root->height = 1 + maxHeight(getNodeHeight(root->left), getNodeHeight(root->right))

int balance = getBalance(root);
```

Se reajusta la altura del nodo después del movimiento que tuvo el árbol y por ende cambia 
la estructura de este.

Se calcula el factor de balance del nodo root del Árbol AVL.

```c++ 
        if (balance > 1 && getBalance(root->left) >= 0)
            return rightRotate(root);

        if (balance > 1 && getBalance(root->left) < 0) {
            root->left = leftRotate(root->left);
            return rightRotate(root);
        }

        if (balance < -1 && getBalance(root->right) <= 0)
            return leftRotate(root);

        if (balance < -1 && getBalance(root->right) > 0) {
            root->right = rightRotate(root->right);
            return leftRotate(root);
        }
```
Estas son las condicionales para re ajustar el árbol en caso de haber un desbalance
en alguna parte del árbol.

- **Primera condicional**: Caso en el que desbalance es hacia la izquierda, y `getBalance(root->left) >= 0`
verifica si el subárbol izquierdo está desbalanceado hacia la izquierda. Como consecuencia se realiza una
rotación a la derecha para equilibrar el árbol.
- **Segunda condicional**: Caso en el que el desbalance es hacia la izquierda y el subárbol izquierdo está
desbalanceado hacia la derecha (izquierda-derecha). Como consecuencia a este subárbol izquierdo se le realiza
una rotación hacia la izquierda y posteriormente al árbol completo se le realiza una rotación a la derecha.
- **Tercera condicional**: Caso en el que el desbalance es hacia la derecha y verifica si el subárbol derecho tiene un desbalance 
la derecha (caso de derecha-derecha). Como consecuencia se realiza una rotación a la izquierda para mantener el balance del árbol AVL.
- **Cuarta condicional**: Caso en el que el desbalance es hacia la derecha y el subárbol derecho tiene un desbalance hacia la izquierda (caso de derecha-izquierda o zig zag).
Como consecuencia de esto, se reajusta el subárbol derecho haciéndole una rotación hacia la derecha, dejando un caso derecha-derecha.
Y posteriormente se realiza la rotación hacia la izquierda del árbol completo para dejarlo balanceado.
Finalmente se retorna el nodo raíz después de todas las modificaciones que tuvo el árbol AVL.

```c++
    void deleteTree(Node* node) {
        if (node == nullptr)
            return;

    deleteTree(node->left);
    deleteTree(node->right);

    delete node;
}
```
Este método solo elimina el árbol o subárbol desde el nodo indicado hasta llegar al caso base de manera recursiva.
Al final de eliminar los nodos hijos, se elimina el nodo proporcionado.

```c++
    public:
        AVLTree() : root(nullptr) {}

        ~AVLTree() {
            deleteTree(root);
        }
```
Continúan los atributos públicos del árbol AVL.

- `AVLTree()`: Constructor por defecto de un árbol AVL con su nodo `root` apuntando a `nulllptr`.
Lo que significa que el árbol se inicializa vacío.
- ~AVLTree(): Destructor del árbol AVL que elimina todos los nodos y libera la memoria para evitar fugas de memoria.
```c++
    void insert(int value) {
        root = insertNode(root, value);
    }
```

Este método inserta un nuevo valor en el árbol manteniendo el equilibro o balance en este.
Llama al método privado insertNode para encontrar la posición correcta y realizar cualquier rotación necesaria para mantener el equilibrio.

```c++
    void remove(int value) {
        root = deleteNode(root, value);
    }
```

Este método elimina un valor del árbol AVL.
Llama al método `deleteNode` para encontrar la posición correcta y realizar rotaciones para mantener el balance del árbol AVL.

```c++
    void inorderTraversal(Node* node) {
        if (node == nullptr)
            return;
        
        inorderTraversal(node->left);
        std::cout << node->value << " ";
        inorderTraversal(node->right);
    }

    
    void preorderTraversal(Node* node) {
        if (node == nullptr)
            return;

        std::cout << node->value << " ";
        preorderTraversal(node->left);
        preorderTraversal(node->right);
    }
    
    void postorderTraversal(Node* node) {
        if (node == nullptr)
            return;
    
        postorderTraversal(node->left);
        postorderTraversal(node->right);
        std::cout << node->value << " ";
    }
```
- `inorderTraversal(Node* node)`: Se realiza un recorrido del árbol de manera ordenada. Visitando el
subárbol izquierdo, el nodo raíz y el subárbol derecho.
Este tipo de recorrido muestra los valores en orden ascendente y es útil para verificar el orden del árbol binario de búsqueda.
- `preorderTraversal(Node* node)`: Se realiza un recorrido pre-order visitando primero el nodo actual, 
luego el subárbol izquierdo y finalmente el subárbol derecho. Esto muestra el árbol dejando el nodo raíz arriba y los subárboles derecho e izquierdo en su orden.
- `postorderTraversal(Node* node)`: Se realiza un recorrido post-order, visita el subárbol izquierdo, luego el derecho y finalmente la raíz.
  Este recorrido es útil para operaciones como eliminación o cálculo de la altura de un árbol.

```c++
    void printTree(Node* root, int space = 0, int COUNT = 10) {
        if (root == nullptr) {
            return;
        }
        
        space += COUNT;
        printTree(root->right, space);
        
        std::cout << std::endl;
        
        for (int i = COUNT; i < space; i++) {
            std::cout << " ";
        }
        
        std::cout << root->value << "\n";
        
        printTree(root->left, space);
    }
```
Este método se encarga de imprimir la estructura del árbol de manera visual,
 mostrando sus nodos y relaciones jerárquicas.
Parámetros:
- `Node* root`: El nodo raíz del árbol o subárbol que se va a imprimir.
- `int space`: Un valor que determina la cantidad de espacios para la indentación. Se utiliza para controlar la distancia entre niveles del árbol.
- `int COUNT`: Define cuántos espacios se agregarán para cada nivel de profundidad en el árbol. El valor predeterminado es 10.
Resto del método:
- `if (root == nullptr)`: Si root es nullptr, el método retorna inmediatamente, ya que no hay nada que imprimir.
- `space += COUNT`:Se incrementa el nivel de indentación para el nodo actual, sumando el valor de COUNT. Esto determina cuántos espacios se agregarán para separar los nodos de niveles diferentes.
- `printTree(root->right, space)`: Primero, se imprime el subárbol derecho, utilizando el nivel de indentación actualizado. Se inserta una nueva línea `(std::cout << std::endl)` para separar cada nivel.
- `for (int i = COUNT; i < space; i++)`: Luego, un bucle for agrega la cantidad de espacios necesaria para lograr la indentación correcta.
- `std::cout << root->value << "\n"`: Imprime el valor del nodo actual con la indentación correcta. Esto muestra la jerarquía del árbol.
- `printTree(root->left, space)`: Después de imprimir el nodo actual, se llama a printTree(root->left, space), para imprimir el subárbol izquierdo.