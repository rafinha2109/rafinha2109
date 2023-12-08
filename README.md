#montar o estoque 
estoque = []
#funcionalidade 01 do projeto (cadastro de produto)
def cadastrar_produto():
    nome = input("Digite o nome do produto: ")
    codigo = int(input("Digite o código do produto: "))
    preco = float(input("Digite o preço sugerido de venda: "))
#condicional para que não sejam aceitos valores negativos no cadastro de produtos
    if preco <= 0: 
        print("Valor indisponivel para cadastro de produto!")
    else:
          quantidade = int(input("Digite a quantidade: ")) 
#colocando o produto dentro do estoque 
    for produto in estoque:
        #condicional para que produtos diferentes não possuam o mesmo codigo 
        if produto['codigo'] == codigo:
            print("Erro! Esse código já exite no sistema.")
            print("Esse código já existe no sistema.")
            print("Você deseja adicionar mais uma quantidade do produto? [s/n]")
            adicionar = input("")
            #atualização do estoque caso o usuario deseje inserir uma quantidade nova do produto sem iniciar o
            #cadastro novamente
            if adicionar == 's':
                produto['quantidade'] += quantidade
                print(f"Quantidade do produto {produto['nome']} atualizada para {produto['quantidade']}.")
                return

    novo_produto = {
        'nome' : nome,
        'codigo' : codigo,
        'preco' : preco,
        'quantidade' : quantidade
    }

    estoque.append(novo_produto)
    print("Novo produto cadastrado com sucesso!")
#funcionalidade 02 do projeto ( tabela de estoque )
def mostrar_tabela():
    #caso o estoque esteja vazio será apresentado ao usuario que não possui estoque disponivel
    if not estoque:
        print("Estoque vazio.")
    else:
        # se tiver itens no estoque irá imprimir uma tabela
        print("* TABELA DE ESTOQUE * ")
        print("Nome          | Código        | Preço         | Quantidade    ")
        for produto in estoque:
            print(f"{produto['nome']:<14}| {produto['codigo']:<14}| {produto['preco']:<14}| {produto['quantidade']:<14}")
#funcionalidade 03 do codigo salvar o arquivo e manter o sistema atualizado
def salvar_arquivo():
    print('-' * 11 + '\nPROCESSANDO\n' + '-' * 11)
    arquivo = open('estoque.txt', 'w')
    arquivo.close()
    arquivo = open('estoque.txt', 'a')
    for v in estoque:
        arquivo.write(f'{v}\n')
    arquivo.close()
    print('O arquivo estoque.txt foi criado e já está atualizado com o estoque!')
#funcionalidade 04 do codigo (fazer a compra)
def fazer_compra():
    #inicia a contagem de clientes e o total de vendas com 0 
    contar_clientes = 0
    total_vendas = 0
# pede o codigo do produto que será inserido ao "carrinho"
    while True:
        total_compra = 0
        pedido = int(input("Digite o código do seu produto (ou digite '0' para finalizar): "))
        if pedido == 0:
            break
        produto_encontrado = False
#se o houver produto inserido no carrinho ocorre a checagem no estoque e ele solicita a quantidade desejada
        for produto in estoque:
            if produto['codigo'] == pedido:
                quantidade = float(input("Informe a quantidade desejada: "))
                if quantidade <= produto['quantidade']:
                    #adiciona o preço do produto e multiplica pela quantidade o que aumenta o valor presente 
                    #carrinho
                    total_compra += produto['preco'] * quantidade
                    produto['quantidade'] -= quantidade
                    produto_encontrado = True
                    #se não houver mais itens desse tipo no estoque ele informa que a quantidade ta indisponivel
                else:
                    print("Quantidade indisponível")
        # informa o valor total da compra e mostra as formas de pagamento disponiveis 
        print(f'O valor total da compra é R${total_compra}.')
        print("1. Dinheiro")
        print("2. Cartão de crédito (à vista)")
        print("3. Cartão de crédito parcelado em até 4x (com juros de 3.75%)")
        opcao = int(input("Escolha a opção de pagamento (1/2/3): "))
        if opcao == 1:
            print(f"Pagamento em dinheiro. Total a pagar: R$ {total_compra}")
            dinheiro = float(input("Digite o valor que será dado: "))
            troco = dinheiro - total_compra
            if dinheiro < total_compra:
                print("Valor insuficiente para realizar a compra")
            else:
                print(" Compra realizada com sucesso!")
                print(f"Você receberá um troco de R${troco}")
        elif opcao == 2:
                print(f"Pagamento em cartão à vista. Total a pagar: R$ {total_compra}")
                cartao=int(input("Digite o número do seu cartão de crédito:"))
                cvc=int(input("Informe o codigo de segurança do cartão"))
                print("Compra Realizada!")
                break
        elif opcao == 3:
            num_parcelas = int(input("Digite o número de parcelas, valor máximo 4: "))
            total_compra = total_compra + total_compra * 0.0375
            parcela = total_compra / num_parcelas
            print(f"O valor total da compra com o juros será: {total_compra}")
            print(f"O valor de cada parcela será: {parcela}")
            cartao_p2= int(input("informe o número do seu cartão de crédito:"))
            cvc_2=int(input("Informe o código de segurança do seu cartão de crédito"))
            print("Compra realizada com sucesso!")
        else:
            print("Opção de pagamento invalida!")
        break
    contar_clientes += 1
    total_vendas += total_compra
    print(total_vendas)
    return contar_clientes, total_vendas
def finalizar_dia():
    print("Finalizado com sucesso") 
    cliente, valor = fazer_compra()
    
    if opcao == 5:
        with open("Finalizar_o_dia.txt", "w") as arquivo:
            arquivo.write("Relatório do dia \n\n\n\n\n")
            arquivo.write(f"O total de clientes que compraram na loja foi {cliente} \n\n\n\n\ ")
            arquivo.write(f"O valor total das vendas do dia foi {valor} \n\n\n\n\n\ ")
            arquivo.write(f"Ralatorio do dia Completo")
            arquivo.close()
def menu():
    print("----- MENU -----")
    print("1. Cadastrar novo produto")
    print("2. Mostrar tabela")
    print("3. Salvar arquivo")
    print("4. Fazer comprar")
    print("5. Finalizar o dia")
    print("6. Sair")

menu()
op = int(input("Escolha uma opção: "))
while op != 0:
    if op == 1:
        cadastrar_produto()
    elif op == 2:
        mostrar_tabela()
    elif op == 3:
        salvar_arquivo()
    elif op == 4:
        fazer_compra()
    elif op == 5:
        pass
    elif op == 6:
        pass
    menu()
    op = int(input("Escolha uma opção: "))

