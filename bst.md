Em uma BST balanceada, as operações de busca, inserção e remoção são realizadas em tempo O(log n), onde n é o número de nós na árvore. No entanto, se a árvore não estiver balanceada, o tempo de busca pode ser pior, O(n) em casos extremos.

```python
class No:
    def __init__(self, valor):
        self.valor = valor
        self.esquerda = None
        self.direita = None

class BST:
    def __init__(self):
        self.raiz = None
    
    def inserir(self, valor):
        self.raiz = self._inserir_valor(self.raiz, valor)
        return self.raiz is not None
    
    def _inserir_valor(self, no, valor):
        if no is None:
            no = No(valor)
        else:
            if valor < no.valor:
                no.esquerda = self._inserir_valor(no.esquerda, valor)
            else:
                no.direita = self._inserir_valor(no.direita, valor)
        return no

    def buscar(self, valor):
        return self._buscar_valor(self.raiz, valor)

    def _buscar_valor(self, no, valor):
        if no is None or no.valor == valor:
            return no is not None
        elif valor < no.valor:
            return self._buscar_valor(no.esquerda, valor)
        else:
            return self._buscar_valor(no.direita, valor)

    def deletar(self, valor):
        self.raiz, deletado = self._deletar_valor(self.raiz, valor)
        return deletado

    def _deletar_valor(self, no, valor):
        if no is None:
            return no, False

        deletado = False
        if valor == no.valor:
            deletado = True
            if no.esquerda and no.direita:
                pai, filho = no, no.direita
                while filho.esquerda is not None:
                    pai, filho = filho, filho.esquerda
                filho.esquerda = no.esquerda
                if pai != no:
                    pai.esquerda = filho.direita
                    filho.direita = no.direita
                no = filho
            elif no.esquerda or no.direita:
                no = no.esquerda or no.direita
            else:
                no = None
        elif valor < no.valor:
            no.esquerda, deletado = self._deletar_valor(no.esquerda, valor)
        else:
            no.direita, deletado = self._deletar_valor(no.direita, valor)
        return no, deletado

    def altura(self):
        return self._altura(self.raiz)

    def _altura(self, no):
        if no is None:
            return 0
        return max(self._altura(no.esquerda), self._altura(no.direita)) + 1

    def valores(self):
    return self._valores(self.raiz, [])

    def _valores(self, no, lista):
        if no is None:
            return lista
        lista = self._valores(no.esquerda, lista)
        lista.append(no.valor)
        lista = self._valores(no.direita, lista)
        return lista
```
