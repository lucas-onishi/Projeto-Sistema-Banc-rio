# Projeto-Sistema-Bancario

import pickle
import os

ARQUIVO_DADOS = "dados.pkl"
SENHA_GERENTE = "Gerenci@_2026"

nome_cliente = "Cliente Sem Nome"
saldo = 0.0
historico = []

if os.path.exists(ARQUIVO_DADOS):
    arquivo = open(ARQUIVO_DADOS, "rb")
    dados = pickle.load(arquivo)
    arquivo.close()
    nome_cliente = dados["nome"]
    saldo = dados["saldo"]
    historico = dados["historico"]
    print("Dados carregados com sucesso!")

print(f"\nBem-vindo ao Banco Messi, {nome_cliente}!")


rodando = True

while rodando == True:

    print("\nMENU PRINCIPAL")

    menu_principal = ["1 - Entrar como Cliente", "2 - Entrar como Gerente", "3 - Salvar dados em disco", "4 - Sair do sistema"]

    for item in menu_principal:
        print(item)

    opcao_principal = input("\nVocê deseja: ")

    if opcao_principal == "1":

        print("\nEntrando no modo cliente...")
        
        no_menu_cliente = True

        while no_menu_cliente == True:
           
            print("\nSelecione uma das opções:")
            

            menu_cliente = [
                "\n1 - Consultar saldo",
                "\n2 - Depositar dinheiro",    
                "\n3 - Sacar dinheiro",
                "\n4 - Simular rendimento da poupança",
                "\n5 - Ver extrato de transações",
                "\n6 - Voltar ao menu principal"
            ]

            for item in menu_cliente:
                print(item)

            opcao = input("\nSelecione uma das opções: ")

            if opcao == "1":

                print("\n            CONSULTA DE SALDO            ")
                print(f"Cliente: {nome_cliente}")
                print(f"Saldo disponível: R$ {saldo}")
                input("\nPressione ENTER para continuar...")

            elif opcao == "2":

                print("\n          DEPÓSITO           ")

                valor_texto = input("\nQual valor você deseja depositar? R$ ")
                valor = float(valor_texto)

                if valor <= 0:
                    print("Valor inválido!")
                    print("Operação cancelada.")
                else:
                    saldo_antes = saldo
                    saldo = saldo + valor

                    print("Depósito realizado com sucesso!")
                    print(f"Valor depositado: R$ {valor}")
                    print(f"Saldo anterior: R$ {saldo_antes}")
                    print(f"Saldo atual: R$ {saldo}")
                    input("\nPressione ENTER para continuar...")

                    registro = f"DEPÓSITO | Valor: R$ {valor} | Saldo anterior: R$ {saldo_antes} | Saldo atual: R$ {saldo}"
                    historico = historico + [registro]

            elif opcao == "3":

                print("\n            SAQUE            ")

                valor_texto = input("Qual valor você deseja sacar? R$ ")
                valor = float(valor_texto)

                if valor <= 0:
                    print("Valor inválido! O valor precisa ser maior que zero.")
                    print("Operação cancelada.")
                elif valor > saldo:
                    print("Saldo insuficiente!")
                    print(f"Você tentou sacar R$ {valor}, mas seu saldo é de R$ {saldo}")
                    print("Operação cancelada.")
                else:
                    saldo_antes = saldo
                    saldo = saldo - valor

                    print("Saque realizado com sucesso!")
                    print(f"Valor sacado: R$ {valor}")
                    print(f"Saldo anterior: R$ {saldo_antes}")
                    print(f"Saldo atual: R$ {saldo}")
                    input("\nPressione ENTER para continuar...")
                    registro = f"SAQUE | Valor: R$ {valor} | Saldo anterior: R$ {saldo_antes} | Saldo atual: R$ {saldo}"
                    historico = historico + [registro]

            elif opcao == "4":

                dolarUS = 3.13235332

                print("\n   SIMULAÇÃO DE RENDIMENTO   ")
                print("   Poupança - Taxa 0.5% a.m  ")

                print(f"\nSaldo inicial: R$ {saldo}")
                print("Taxa mensal: 0.5%")
                print("Período: 12 meses")
                print(" Mês | Rendimento | Saldo ")

                saldo_simulado = saldo
                total_rendido = 0.0
                meses = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]

                for mes in meses:

                    rendimento_mes = saldo_simulado * 0.005
                    saldo_simulado = saldo_simulado + rendimento_mes
                    total_rendido = total_rendido + rendimento_mes

                    print(f" {mes}   | R$ {round(rendimento_mes, 2)}  | R$ {round(saldo_simulado, 2)}")

                print(f"Saldo inicial: R$ {saldo}")
                print(f"Total que rendeu: R$ {round(total_rendido, 2)}")
                print(f"Saldo final estimado: R$ {round(saldo_simulado, 2)}")
                input("\nPressione ENTER para continuar...")

            elif opcao == "5":

                print("\n          EXTRATO            ")

                if len(historico) == 0:
                    print("Você ainda não fez nenhuma transação.")

                else:
                    ultimas = []
                    i = len(historico) - 1
                    contador = 0

                    while i >= 0 and contador < 10:
                        ultimas = ultimas + [historico[i]]
                        i = i - 1
                        contador = contador + 1

                    ultimas_ordem = []
                    j = len(ultimas) - 1
                    while j >= 0:
                        ultimas_ordem = ultimas_ordem + [ultimas[j]]
                        j = j - 1

                    print(f"Mostrando {len(ultimas_ordem)} transação(ões):\n")

                    for transacao in ultimas_ordem:
                        print("-", transacao)

                    if len(historico) > 10:
                        print("\n(Mostrando apenas as 10 últimas transações)")

                

            elif opcao == "6":

                print("\nVoltando ao menu principal...")
                no_menu_cliente = False

            elif opcao == "32":

                print("Dólar hoje => R$ 4.98721923")

            else:

                print("Opção inválida! Por favor digite um número entre 1 e 6.")

    elif opcao_principal == "2":

        print("\n       ACESSO DO GERENTE     ")
        print("Você precisa de senha para entrar.")


        tentativas = 0
        autenticado = False

        while tentativas < 3:

            senha_digitada = input("\nDigite a senha do gerente: ")

            print("")

            if senha_digitada == SENHA_GERENTE:
                autenticado = True
                tentativas = 3
                print("Senha correta! Bem-vindo, gerente.")
            else:
                tentativas = tentativas + 1

                if tentativas < 3:
                    print("Senha errada!")
                    print("Tentativas restantes:", 3 - tentativas)
                else:
                    print("Senha errada!")
                    print("Você errou 3 vezes. Acesso bloqueado!")

        if autenticado == True:

            no_menu_gerente = True

            while no_menu_gerente == True:

                print("\n        MENU GERENTE         ")

                menu_gerente = [
                    "\n1 - Alterar nome do cliente",
                    "\n2 - Corrigir saldo do cliente",
                    "\n3 - Ver dados do cliente",
                    "\n4 - Ver extrato do cliente",
                    "\n5 - Voltar ao menu principal"
                ]

                for item in menu_gerente:
                    print(item)

                opcao_gerente = input("\nDigite a opção desejada: ")

                if opcao_gerente == "1":
                    print("\n     ALTERAR NOME CLIENTE    ")
                    print(f"Nome atual: {nome_cliente}")


                    novo_nome = input("\nDigite o novo nome do cliente: ")
                    nome_cliente = novo_nome

                    print("\nNome atualizado com sucesso!")
                    print(f"Novo nome: {nome_cliente}")

                elif opcao_gerente == "2":

                    print("\n       CORRIGIR SALDO        ")
                    print(f"Cliente: {nome_cliente}")
                    print(f"Saldo atual: R$ {saldo}")

                    novo_saldo_texto = input("\nDigite o novo saldo: R$ ")
                    novo_saldo = float(novo_saldo_texto)

                    saldo_antes = saldo
                    saldo = novo_saldo

                    registro = f"CORREÇÃO PELO GERENTE | Saldo anterior: R$ {saldo_antes} | Novo saldo: R$ {saldo}"
                    historico = historico + [registro]

                    print("\nSaldo corrigido com sucesso!")
                    print(f"Saldo anterior: R$ {saldo_antes}")
                    print(f"Saldo novo: R$ {saldo}")

                elif opcao_gerente == "3":

                    print("\n      DADOS DO CLIENTE       ")
                    print(f"Nome do cliente: {nome_cliente}")
                    print(f"Saldo atual: R$ {saldo}")
                    print(f"Total de transacoes: {len(historico)}")
                    input("\nPressione ENTER para continuar...")

                elif opcao_gerente == "4":

                    print("\n     EXTRATO DO CLIENTE      ")


                    if len(historico) == 0:

                        print("Nenhuma transação registrada ainda.")
                        input("\nPressione ENTER para continuar...")
                    else:

                        print("Últimas transações (mais recentes primeiro):")
                        input("\nPressione ENTER para continuar...")
                        ultimas_gerente = []
                        i = len(historico) - 1
                        contador = 0

                        while i >= 0 and contador < 10:
                            ultimas_gerente = ultimas_gerente + [historico[i]]
                            i = i - 1
                            contador = contador + 1

                        for transacao in ultimas_gerente:
                            print(f"- {transacao}")

                        if len(historico) > 10:
                            print("(Mostrando apenas as 10 últimas)")
    
                elif opcao_gerente == "5":

                    print("Voltando ao menu principal...")
                    no_menu_gerente = False

                else:

                    print("Opção inválida! Digite um número entre 1 e 5.")

    elif opcao_principal == "3":

        print("\n       SALVANDO DADOS        ")
        print("Salvando informacoes do cliente...")

        saldo_garantia_pos_transacional = "Ativado"

        dados = {}
        dados["nome"] = nome_cliente
        dados["saldo"] = saldo
        dados["histórico"] = historico
        dados["Saldo Garantia Pos-Transacional"] = saldo_garantia_pos_transacional

        arquivo = open(ARQUIVO_DADOS, "wb")
        pickle.dump(dados, arquivo)
        arquivo.close()

        print(f"Nome salvo: {nome_cliente}")
        print(f"Saldo salvo: R$ {saldo}")
        print(f"Transações salvas: {len(historico)}")
        print("Dados salvos com sucesso!")


    elif opcao_principal == "4":

        print("\n          SAINDO...          ")
        print("Salvando dados antes de sair...")

        saldo_garantia_pos_transacional = "Ativado"

        dados = {}
        dados["nome"] = nome_cliente
        dados["saldo"] = saldo
        dados["histórico"] = historico
        dados["Saldo Garantia Pos-Transacional"] = saldo_garantia_pos_transacional

        arquivo = open(ARQUIVO_DADOS, "wb")
        pickle.dump(dados, arquivo)
        arquivo.close()

        print("\nDados salvos!")
        print("Obrigado por usar o Banco Messi!")
        print(f"Até logo, {nome_cliente}!")

        rodando = False

    else:

        print("Opção inválida! Por favor digite um número entre 1 e 4.")
