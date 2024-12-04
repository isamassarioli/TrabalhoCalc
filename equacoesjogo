import tkinter as tk
from tkinter import messagebox
import random
import json

# Função para gerar uma equação de primeiro grau
def gerar_equacao_primeiro_grau():
    a = random.randint(1, 10)  # Coeficiente de x
    b = random.randint(0, 10)  # Termo constante
    x = random.randint(1, 10)  # Valor de x (resposta correta)
    resultado = a * x + b      # Calcula o resultado
    return f"{a}x + {b} = {resultado}", x  # Retorna a equação e o valor de x

# Função para gerar uma equação de segundo grau
def gerar_equacao_segundo_grau():
    a = random.randint(1, 10)  # Coeficiente de x²
    b = random.randint(0, 10)  # Coeficiente de x
    c = random.randint(0, 10)  # Termo constante
    x = random.randint(1, 10)  # Valor de x (resposta correta)
    resultado = a * x**2 + b * x + c  # Calcula o resultado
    return f"{a}x^2 + {b}x + {c} = {resultado}", x  # Retorna a equação e o valor de x

# Função para verificar a resposta do jogador
def verificar_resposta():
    global pontuacao, tempo_restante    # Define as variáveis como globais para poderem ser acessadas e modificadas
    resposta = entrada_resposta.get()   # Obtém o valor digitado pelo jogador
    try:
        x = float(resposta) # Converte a resposta para número
        if x == valor_x:    # Verifica se está correta
            label_resultado.config(text="Correto!", fg="green")
            pontuacao += 1  # Incrementa a pontuação
            label_pontuacao.config(text=f"Pontuação: {pontuacao}")
            iniciar_fase()  # Inicia nova fase
        else:
            label_resultado.config(text="Errado!", fg="red")
            tempo_restante -= 5  # Diminui o tempo em 5 segundos para cada resposta errada
            if tempo_restante < 0:
                tempo_restante = 0
            label_tempo.config(text=f"Tempo restante: {tempo_restante}s")
    except ValueError:
        label_resultado.config(text="Por favor, insira um número válido.", fg="red")

# Função para iniciar uma nova fase
def iniciar_fase():
    global valor_x, nivel   # Define as variáveis como globais para poderem ser acessadas e modificadas
    nivel = random.choice([1, 2])   # Escolhe aleatoriamente o nível da equação
    if nivel == 1:  # Gera uma equação de primeiro grau
        equacao, valor_x = gerar_equacao_primeiro_grau()
    else:        # Gera uma equação de segundo grau
        equacao, valor_x = gerar_equacao_segundo_grau()
    label_funcao.config(text=f"Resolva para x: {equacao}")
    entrada_resposta.delete(0, tk.END)  # Limpa o campo de resposta
    label_resultado.config(text="")

# Função para iniciar o jogo
def iniciar_jogo():
    global tempo_restante, pontuacao, nome_jogador
    nome_jogador = entrada_nome.get()  # Obtém o nome do jogador
    if not nome_jogador:
        messagebox.showwarning("Aviso", "Por favor, insira seu nome.")
        return
    tempo_restante = 120  # Reseta o tempo
    pontuacao = 0  # Reseta a pontuação
    label_pontuacao.config(text=f"Pontuação: {pontuacao}")
    atualizar_tempo()  # Inicia o temporizador
    iniciar_fase()  # Começa o jogo

# Função para atualizar o temporizador
def atualizar_tempo():
    global tempo_restante
    if tempo_restante > 0:
        tempo_restante -= 1 # Diminui o tempo
        label_tempo.config(text=f"Tempo restante: {tempo_restante}s")
        root.after(1000, atualizar_tempo)   # Chama novamente após 1 segundo
    else:
        label_funcao.config(text="Tempo esgotado! Clique em 'Iniciar' para jogar novamente.")
        entrada_resposta.delete(0, tk.END)  # Limpa o campo de resposta
        salvar_pontuacao()  # Salva a pontuação do jogador

# Função para parar o jogo
def parar_jogo():
    global tempo_restante
    tempo_restante = 0
    label_funcao.config(text="Jogo parado. Clique em 'Iniciar' para jogar novamente.")
    entrada_resposta.delete(0, tk.END)
    salvar_pontuacao()

# Função para salvar a pontuação do jogador
def salvar_pontuacao():
    global nome_jogador, pontuacao
    try:
        with open('ranking.json', 'r') as f:    # Tenta abrir o arquivo de ranking
            ranking = json.load(f)  # Carrega o conteúdo do arquivo
    except FileNotFoundError:
        ranking = []    # Se o arquivo não existir, cria uma lista vazia

    ranking.append({'nome': nome_jogador, 'pontuacao': pontuacao})  # Adiciona a pontuação do jogador à lista
    
    with open('ranking.json', 'w') as f:    # Abre o arquivo para escrita
        json.dump(ranking, f)   # Salva a lista no arquivo

# Função para exibir o ranking dos jogadores
def exibir_ranking():
    try:
        with open('ranking.json', 'r') as f:
            ranking = json.load(f)
            ranking.sort(key=lambda x: x['pontuacao'], reverse=True)    # Ordena o ranking pela pontuação
            ranking_texto = "\n".join([f"{item['nome']}: {item['pontuacao']}" for item in ranking]) # Formata o texto
            messagebox.showinfo("Ranking", ranking_texto)   # Exibe o ranking em uma janela de diálogo
    except FileNotFoundError:
        messagebox.showinfo("Ranking", "Nenhum jogador registrado ainda.")  # Se o arquivo não existir, exibe uma mensagem

# Configuração da interface gráfica
root = tk.Tk()  # Cria a janela principal
root.title("Caça às Equações")  # Define o título da janela
root.geometry("400x400")    # Define o tamanho da janela
root.configure(bg="#f0f0f0")    # Define a cor de fundo da janela

# Estilo dos widgets
estilo_label = {"font": ("Arial", 12), "bg": "#f0f0f0"}     # Define o estilo dos labels
estilo_entrada = {"font": ("Arial", 12)} 
estilo_botao = {"font": ("Arial", 12), "bg": "#4CAF50", "fg": "white", "activebackground": "#45a049"}

# Cria e configura o label para o nome do jogador
label_nome = tk.Label(root, text="Digite seu nome:", **estilo_label)    
label_nome.pack(pady=5)

# Cria e configura a entrada de texto para o nome do jogador
entrada_nome = tk.Entry(root, **estilo_entrada)
entrada_nome.pack(pady=5)

# Cria e configura o label para a função a ser resolvida
label_funcao = tk.Label(root, text="Clique em 'Iniciar' para começar", **estilo_label)
label_funcao.pack(pady=10)

# Cria e configura a entrada de texto para a resposta do jogador
entrada_resposta = tk.Entry(root, **estilo_entrada)
entrada_resposta.pack(pady=5)

# Cria e configura o botão para verificar a resposta do jogador
botao_verificar = tk.Button(root, text="Verificar", command=verificar_resposta, **estilo_botao)
botao_verificar.pack(pady=5)

# Cria e configura o botão para iniciar o jogo
botao_iniciar = tk.Button(root, text="Iniciar", command=iniciar_jogo, **estilo_botao)
botao_iniciar.pack(pady=5)

# Cria e configura o botão para parar o jogo
botao_parar = tk.Button(root, text="Parar", command=parar_jogo, **estilo_botao)
botao_parar.pack(pady=5)

# Cria e configura o label para mostrar o resultado da verificação da resposta
label_resultado = tk.Label(root, text="", **estilo_label)
label_resultado.pack(pady=10)

# Cria e configura o label para mostrar o tempo restante
label_tempo = tk.Label(root, text="Tempo restante: 120s", **estilo_label)
label_tempo.pack(pady=5)

# Cria e configura o label para mostrar a pontuação do jogador
label_pontuacao = tk.Label(root, text="Pontuação: 0", **estilo_label)
label_pontuacao.pack(pady=5)

# Cria e configura o botão para exibir o ranking dos jogadores
botao_ranking = tk.Button(root, text="Exibir Ranking", command=exibir_ranking, **estilo_botao)
botao_ranking.pack(pady=5)

# Define o tempo restante do jogo em 120 segundos
tempo_restante = 120
# Inicializa a pontuação do jogador em 0
pontuacao = 0

# Inicia o loop principal da interface gráfica
root.mainloop()
