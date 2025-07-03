# Projeto de Programação 
SISTEMA DE AVALIAÇÃO DE RESTAURANTES NA REGIÃO 

/*
 * Funcionalidades:
 * 1. Avaliacao de restaurante (nota sobre o estabelecimento)
 * 2. Listar todos os restaurantes cadastrados
 * 3. Editar cadastros existentes
 * 4. Excluir cadastros com confirmação
 */

#include <stdio.h>  // Biblioteca padrao para entrada e saida (ex: printf, scanf)

#define MAX 100  // Define o número maximo de cadastros permitidos

// Estrutura que representa uma pessoa com código, nome, idade e nota
struct Pessoa {
    int codigo;
    char nome[50];  // String de até 49 caracteres + 1 para o caractere '\0' de finalizacao
    int idade;
    int nota;       // Nota de avaliação do restaurante
};

// Vetor que armazena até 100 avaliacoes de restaurantes
struct Pessoa pessoas[MAX];
// Variável que armazena quantas pessoas já deixaram sua opinião
int qtd = 0;

// Funcao que cadastra uma nova avaliacao
void cadastrar() {
    // Verifica se o limite de cadastros foi atingido
    if (qtd >= MAX) {
        printf("\nLimite de avaliacoes atingido (%d cadastros)!\n", MAX);
        return; // Encerra a funcao
    }

    printf("\n--- Nova avaliacao ---\n");

    // Pede os dados do restaurante: avaliacao
    printf("data da avaliacao: ");
    scanf("%d", &pessoas[qtd].codigo);
    while (getchar() != '\n'); // Limpa o buffer do teclado

    printf("Nome: ");
    scanf(" %49[^\n]", pessoas[qtd].nome); // Lê até 49 caracteres com espacos

    printf("Idade: ");
    scanf("%d", &pessoas[qtd].idade);

    printf("avaliacao (1 a 10): ");
    scanf("%d", &pessoas[qtd].nota);

    // Aumenta o número de restaurantes cadastrados
    qtd++;

    printf("\nCadastro realizado com sucesso!\n");
}

// Funcao que busca um restaurante pelo código informado
int buscarPorCodigo(int codigo) {
    int i;
    for (i = 0; i < qtd; i++) {
        if (pessoas[i].codigo == codigo) {
            return i; // Retorna o índice da pessoa no vetor
        }
    }
    return -1; // Retorna -1 se nao encontrar
}

// Funcao que edita os dados de um restaurante
void editar() {
    int codigo, index;

    printf("\n--- EDITAR CADASTRO ---\n");
    printf("Digite o código do restaurante: ");
    scanf("%d", &codigo);

    // Procura a avaliacao com o codigo digitado
    index = buscarPorCodigo(codigo);

    // Se nao encontrar, avisa e retorna
    if (index == -1) {
        printf("\nAvaliacao nao encontrada!\n");
        return;
    }

    // Mostra os dados atuais
    printf("\nEDITANDO CADASTRO:\n");
    printf("Codigo: %d\n", pessoas[index].codigo);
    printf("Nome atual: %s\n", pessoas[index].nome);
    printf("Idade atual: %d\n", pessoas[index].idade);
    printf("Nota atual: %d\n\n", pessoas[index].nota);

    // Pede os novos dados
    printf("Novo nome: ");
    scanf(" %49[^\n]", pessoas[index].nome);

    printf("Nova idade: ");
    scanf("%d", &pessoas[index].idade);

    printf("Nova nota (1 a 5): ");
    scanf("%d", &pessoas[index].nota);

    printf("\nCadastro atualizado com sucesso!\n");
}

// Funcao que exclui uma avaliacao do sistema
void excluir() {
    int codigo, index;
    char confirmacao;

    printf("\n--- EXCLUIR CADASTRO ---\n");
    printf("Digite o código da pessoa: ");
    scanf("%d", &codigo);

    index = buscarPorCodigo(codigo);

    if (index == -1) {
        printf("\nAvaliacao nao encontrada!\n");
        return;
    }

    // Mostra os dados do restaurante e pede confirmacao
    printf("\nCONFIRMAR EXCLUSÃO:\n");
    printf("Código: %d\n", pessoas[index].codigo);
    printf("Nome: %s\n", pessoas[index].nome);
    printf("Idade: %d\n", pessoas[index].idade);
    printf("Nota: %d\n", pessoas[index].nota);

    printf("\nTem certeza que deseja excluir? (s/n): ");
    scanf(" %c", &confirmacao);

    // Se confirmar, remove a avaliacao do vetor e ajusta a quantidade
    if (confirmacao == 's' || confirmacao == 'S') {
        int i;
        for (i = index; i < qtd - 1; i++) {
            pessoas[i] = pessoas[i + 1]; // Desloca os dados para "tapar o buraco"
        }
        qtd--; // Diminui a quantidade total
        printf("\nCadastro excluído com sucesso!\n");
    } else {
        printf("\nOperação cancelada.\n");
    }
}

// Funcao que lista todos os restaurantes cadastrados
void listar() {
    printf("\n--- RESTAURANTES CADASTRADOS ---\n");
    printf("Total: %d\n\n", qtd);

    if (qtd == 0) {
        printf("Nenhum restaurante cadastrado.\n");
        return;
    }

    // Cabeçalho da tabela
    printf("CÓDIGO  RESTAURANTE                                       AVALIACAO\n");
    printf("-------------------------------------------------------------\n");

    // Mostra os dados dos restaurantes
    int i;
    for (i = 0; i < qtd; i++) {
        printf("%-8d%-50s%-8d\n",
               pessoas[i].codigo,
               pessoas[i].nome,
               pessoas[i].nota);
    }
}

// Funcao principal (ponto de entrada do programa)
int main() {
    int op;

    printf("\n----------------------------------------");
    printf("\n    SISTEMA DE CADASTRO DE AVALIACAO DE RESTAURANTE");
    printf("\n----------------------------------------\n");

    // Loop principal do menu
    do {
        printf("\n----------- MENU PRINCIPAL -----------\n");
        printf("1. Cadastrar uma nova avaliacao\n");
        printf("2. Listar todas as avaliacoes existente\n");
        printf("3. Editar cadastro existente\n");
        printf("4. Excluir cadastro\n");
        printf("0. Sair do sistema\n");
        printf("--------------------------------------\n");
        printf("Opcao: ");
        scanf("%d", &op);
        while (getchar() != '\n'); // Limpa o buffer do teclado

        // Executa a opcao escolhida
        switch (op) {
            case 1: cadastrar(); break;
            case 2: listar(); break;
            case 3: editar(); break;
            case 4: excluir(); break;
            case 0: printf("\nEncerrando sistema...\n"); break;
            default: printf("\nOpcao invalida! Tente novamente.\n");
        }

    } while (op != 0); // Repete até o usuário escolher sair

    printf("\nOBRIGADO POR AVALIAR NO NOSSO SITE! VOLTE SEMPRE <3 !\n");
    return 0; // Finaliza o programa
}

