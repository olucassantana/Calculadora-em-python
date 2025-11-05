

✅ Recursos incluídos:
Interface gráfica limpa e moderna (modo escuro)
Operações básicas: + - * / %
Parênteses, ponto decimal e potência (x²)
Raiz quadrada (√)
Constante π (pi)
Botão de apagar (⌫)
Botão “C” para limpar tudo
Avaliação segura usando apenas funções do math
Layout adaptável (redimensionamento automático das células)

import tkinter as tk
from tkinter import messagebox
import math

# -----------------------------
# Funções da Calculadora
# -----------------------------
class Calculadora:
    def __init__(self, root):
        self.root = root
        self.root.title("Calculadora")
        self.root.geometry("340x480")
        self.root.resizable(False, False)
        self.root.config(bg="#1e1e1e")

        self.expression = ""
        self.input_text = tk.StringVar()

        self.criar_interface()

    # Atualiza o campo de entrada
    def press(self, item):
        self.expression += str(item)
        self.input_text.set(self.expression)

    # Limpa tudo
    def clear(self):
        self.expression = ""
        self.input_text.set("")

    # Apaga o último caractere
    def delete(self):
        self.expression = self.expression[:-1]
        self.input_text.set(self.expression)

    # Calcula o resultado
    def equal(self):
        try:
            result = str(eval(self.expression, {"__builtins__": None}, math.__dict__))
            self.input_text.set(result)
            self.expression = result
        except Exception as e:
            messagebox.showerror("Erro", "Expressão inválida")
            self.expression = ""
            self.input_text.set("")

    # Cria todos os elementos gráficos
    def criar_interface(self):
        entrada = tk.Entry(
            self.root,
            textvariable=self.input_text,
            font=('Arial', 24),
            bg="#2e2e2e",
            fg="white",
            bd=0,
            justify="right",
            insertbackground="white"
        )
        entrada.grid(row=0, column=0, columnspan=4, ipadx=8, ipady=20, pady=10, padx=10, sticky="nsew")

        # Botões
        botoes = [
            ('C', 1, 0, self.clear), ('⌫', 1, 1, self.delete), ('%', 1, 2, lambda: self.press('%')), ('/', 1, 3, lambda: self.press('/')),
            ('7', 2, 0, lambda: self.press('7')), ('8', 2, 1, lambda: self.press('8')), ('9', 2, 2, lambda: self.press('9')), ('*', 2, 3, lambda: self.press('*')),
            ('4', 3, 0, lambda: self.press('4')), ('5', 3, 1, lambda: self.press('5')), ('6', 3, 2, lambda: self.press('6')), ('-', 3, 3, lambda: self.press('-')),
            ('1', 4, 0, lambda: self.press('1')), ('2', 4, 1, lambda: self.press('2')), ('3', 4, 2, lambda: self.press('3')), ('+', 4, 3, lambda: self.press('+')),
            ('0', 5, 0, lambda: self.press('0')), ('.', 5, 1, lambda: self.press('.')), ('(', 5, 2, lambda: self.press('(')), (')', 5, 3, lambda: self.press(')')),
            ('√', 6, 0, lambda: self.press('sqrt(')), ('x²', 6, 1, lambda: self.press('**2')), ('π', 6, 2, lambda: self.press('pi')), ('=', 6, 3, self.equal)
        ]

        for (texto, linha, coluna, comando) in botoes:
            b = tk.Button(
                self.root,
                text=texto,
                command=comando,
                bg="#3a3a3a" if texto not in ('C', '=', '⌫') else ("#f44336" if texto == 'C' else "#4caf50" if texto == '=' else "#ff9800"),
                fg="white",
                font=('Arial', 16, 'bold'),
                relief='flat',
                activebackground="#555",
                activeforeground="white"
            )
            b.grid(row=linha, column=coluna, ipadx=10, ipady=15, padx=5, pady=5, sticky="nsew")

        # Tornar as células elásticas
        for i in range(7):
            self.root.grid_rowconfigure(i, weight=1)
        for j in range(4):
            self.root.grid_columnconfigure(j, weight=1)


# -----------------------------
# Execução principal
# -----------------------------
if __name__ == "__main__":
    root = tk.Tk()
    app = Calculadora(root)
    root.mainloop()
