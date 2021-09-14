/*Disciplina: Técnicas de Programação I 			                  Projeto: Sistema de RH da UFRA
Curso:	Sistemas de Informação                                   		     Professor: Edvar Oliveira
Aluna:                                                                                                   Data: 14 / 04 / 2021
	1. Allana Luiza Silva de Souza – Matricula:  2020037244 
	2. Brunna Danielle Santos de Sousa - Matricula:  2020037262                    
                            

PROJETO REFERENTE AO NAP 02
*/




/*

    Desenvolva um sistema utilizando a Linguagem C que atenda os requisitos abaixo listados

    - Sistema de gerenciamento de recursos humanos.
    - Atributos a serem armazenados dos funcionarios: matricula,nome, email, data de admissao e salario
    - Funcionalidades:
        1. Menu - onde o usuario pode escolher as opcoes de cadastraer, listar, excluir, alterar ou sair
        2. cadastrar - opcao que habilita ao usuario do sistema cadastrar todos os atributos acima listados, 
            porem antes de cadastrar (registrar no arquivo) deve ser feito uma consulta se o novo usuario ja esta cadastrado no arquivo
        3. Listar - listar todos os atributos cadastrados no arquivo
        4. excluir - excluir um usuario do arquivo com base no atributo chave (matricula)
        5. alterar - alterar dados de um determinado usuario escolhido pelo atributo chave, 
            porem o sistema nao deve permitir a alteracao do atributo matricula
        6. sair - opcao de sair do sistema
    - sempre que o usuario escolher uma opcao deve ter a possibilidade de voltar para a tela menu, execeto quando escolher sair do sistema
    

*/
#include<stdlib.h>
#include<stdio.h>
#include<conio.h>
#include<locale.h>
#include<string.h>

typedef struct data DATA; // definição do struct e variaveis
struct data{
  int dia;
  int mes;
  int ano;
};

typedef struct contato CONTATO; // definição do struct e variaveis 
struct contato{
  char nome[40];
  char matricula[20];
  char email[20];
  char salario[10];
  DATA  admissao;
}; 

void cabecalho();    // as funções que o switch case deve chamar para a inicialização do menu
void desenvolvedoras();  
void cadastrar();
void listar();
void verificar();
void editar();
void remover();

main (){  //criar o meu
  setlocale(LC_ALL, "Portuguese");  // habilitar os acentos da lingua 
  int opcao;

  do{
    cabecalho();
    printf(" --------------- MENU -----------------\n\n");
    printf ("1 - Cadastrar\n");
    printf ("2 - Verificar (se a matrícula ja está cadastrada)\n");  // para não cadastrar quem ja esta cadastrada e dar erro
    printf ("3 - Listar\n");
    printf ("4 - Excluir\n");
    printf ("5 - Alterar\n");
    printf ("6 - Sobre\n");
    printf ("0 - Sair\n\n");
    printf("Escolha uma opção\n");
    scanf ("%d",&opcao);  // receber a função que o usuario deseja executar
    
    switch (opcao){
      case 1:
        cadastrar();
      break;
      
      case 2:
        verificar();
      break;
      
      case 3:
        listar();
      break;  
      
      
      case 4:
        remover();
      break;
      
      case 5:
        editar();
      break;
      
      case 6:
        desenvolvedoras();
      break;
      
      case 0:
        printf ("Obrigada por sua visita!\n");
        getch();
      break;
      
      default:
        printf ("Opção Inválida!\n");
        getch();
      break;
    }   
  }while (opcao!=0);  // para saber quando o usuario deseja encerrar o programa 
}

void cabecalho(){
  system ("cls"); // para limpar a limpar a tela de saída de programa que é executado
  printf(" ----------------------------------------\n\n");
  printf("Olá, Bem vindo ao sistema de RH da UFRA\n\n");

}

void desenvolvedoras(){ // dados de quem fez o codigo
  system ("cls");
  
  printf("------------ O programa foi desenvolvido por: -------------\n\n");
  
  printf("Nome: Allana Luiza Silva de Souza\n");
  printf("Email: allanasouzaa33@gmail.com\n");
  printf("Matrícula: 2020037244  \n");
  printf("Data de admissão: 2020.1 \n");
  printf("-----------------------------------------------\n\n");
  
  printf("Nome: Brunna Danielle Santos de Sousa\n");
  printf("Email: brunnasd19@gmail.com\n");
  printf("Matrícula: 2020037262 \n");
  printf("Data de admissão: 2020.1 \n");
  printf("-----------------------------------------------\n\n");
  
  printf("Para voltar ao menu digite 0\n");
  getch();  // espera que o usuário digite uma tecla e parar retornar, ou seja, pausar o terminal

  
}

void cadastrar(){
  FILE* arquivo;  // criando um ponteiro pro arquivo
  CONTATO ctt;
  
  arquivo = fopen("cadastro.txt", "ab");  //abrir o arquivo
  
  if (arquivo == NULL){
    printf("Problemas na abertura do arquivo!\n");  // caso dê erro na abertura do arquivo
  } else{
    
    do{
      system("cls");  // para limpar a limpar a tela de saída de programa que é executado
      fflush(stdin);  // para limpar o buffer do teclado
      printf("Digite o seu nome completo: ");
      gets(ctt.nome);
      
      fflush(stdin);
      printf("Digite o seu email: ");
      gets(ctt.email);
      
      fflush(stdin);
      printf("Digite o sua matricula: ");
      gets(ctt.matricula);
      
      fflush(stdin);
      printf("Digite o seu salario: ");
      gets(ctt.salario);
      
      printf("Digite sua data de admissão (utilize o espaço)");
      scanf("%d %d %d", &ctt.admissao.dia, &ctt.admissao.mes, &ctt.admissao.ano);
      
      fwrite(&ctt, sizeof(CONTATO), 1, arquivo); //para escrever no arquivo
      
      printf("Deseja ccadastrar mais alguém? (s/n)?\n");
            
    }while (getche() == 's' );  // enquanto a opção for igual a "s" o programa vai continuar rodando
  } fclose(arquivo);   // fechar o arquizo
    
}

void listar(){
  FILE* arquivo;   
  CONTATO ctt;
  
  arquivo = fopen("cadastro.txt", "rb"); //abrir em modo rb leitura binaria
  
  if (arquivo == NULL){
    printf("Problemas na abertura do arquivo!\n");    // caso dê erro na abertura do arquivo
  } else{
  
    while( fread(&ctt, sizeof(CONTATO), 1, arquivo)==1){    // enquanto o fread conseguir retornar uma linha
      printf("Nome: %s\n", ctt.nome);
      printf("Matricula: %s\n", ctt.matricula);
      printf ("Email: %s\n", ctt.email);
      printf("Salario: %s\n", ctt.salario);
      printf("Data de admissão: %d/%d/%d\n", ctt.admissao.dia, ctt.admissao.mes, ctt.admissao.ano);
      printf("-----------------------------------------------\n\n");
    }
  }
  printf("Para voltar ao menu digite 0\n");
  fclose(arquivo);
  getch();  // espera que o usuário digite uma tecla e parar retornar, ou seja, pausar o terminal
}

void verificar(){
  FILE* arquivo;
  CONTATO ctt;
  char matricula1[30];
  
  arquivo = fopen("cadastro.txt", "r+b");
  
  if (arquivo == NULL){
    printf("Problemas na abertura do arquivo!\n");    // caso dê erro na abertura do arquivo
  } 
  else{
    fflush(stdin);     // para limpar o buffer do teclado
    printf("Digite a matrícula que você deseja vefificar se consta no sistema:");
    gets(matricula1);  // capturar as matriculas e recuperar as mesmas
    
    
    while(fread(&ctt, sizeof(CONTATO), 1, arquivo)==1){   // enquanto o fread conseguir retornar uma linha, logo o programa cpnseguiu ler essa linha 
      if(strcmp(matricula1,ctt.matricula) == 0){   // comparar a matricula que recebeu com as matriculas ja listadas, se for igual retorna 0
      
        printf("\n----------A matricula já está cadastrada com os dados---------\n");
      
        printf("Nome: %s\n", ctt.nome);
        printf("Matricula: %s\n", ctt.matricula);
        printf ("Email: %s\n", ctt.email);
        printf("Salario: %s\n", ctt.salario);
        printf("Data de admissão: %d/%d/%d\n", ctt.admissao.dia, ctt.admissao.mes, ctt.admissao.ano);
        printf("-----------------------------------------------\n\n");
      }else{
        printf("\nA matricula não consta no sistema, volte ao menu e cadastre :D\n");
        printf("------------------------------------------------------------------------\n\n");
      }
      
    }
  }
  printf("Para voltar ao menu digite 0\n"); 
  fclose(arquivo);
  getch(); 
}
  

void editar(){
    FILE* temp;
    FILE* arquivo;
  CONTATO ctt;
  char matricula2[30];

  arquivo = fopen("cadastro.txt","rb"); //abrir em modo rb leitura binaria
  temp = fopen("tmp.txt","wb");   //abrir em modo wb ele limpa e grava binario

    if(arquivo == NULL && temp == NULL)
    {
      printf("Problemas na abertura do arquivo!\n");
      getch();
    }
    else
    {
      fflush(stdin);
      printf("Digite a matrícula do usuário em que pertende editar os dados: \n ");
      gets(matricula2);

      while(fread(&ctt, sizeof(CONTATO), 1, arquivo) == 1)  //gravando os dados no arquivo
      {
          if(strcmp(matricula2,ctt.matricula) == 0)    // comparar a matricula que recebeu com as matriculas ja listadas, se for igual retorna 0
          {
            printf("Nome: %s\n", ctt.nome);
        printf("Matricula: %s\n", ctt.matricula);
        printf ("Email: %s\n", ctt.email);
        printf("Salario: %s\n", ctt.salario);
        printf("Data de admissão: %d/%d/%d\n", ctt.admissao.dia, ctt.admissao.mes, ctt.admissao.ano);
        printf("-----------------------------------------------\n\n");
          }
          else
          {
           
              fwrite(&ctt, sizeof(CONTATO), 1, temp);    //permite a escrita de um bloco no arquivo temp
          }
      }
        fclose(arquivo);
        fclose(temp);
        fflush(stdin);
          
    if(strcmp(matricula2,ctt.matricula) == 0){
      printf("Tem a certeza que pertende alterar os dados do usuário(s/n)?\n ");
          
        if(getche() == 's')
        {
            if(remove("cadastro.txt") == 0 && rename ("tmp.txt", "cadastro.txt") == 0)  //verifica se as operacoes foram realizadas
                {
                  FILE* arquivo;
            CONTATO ctt;
            
            arquivo = fopen("cadastro.txt", "ab");   // atualiza dos dados do usuario no cadastro
                  
                  printf("\n------------Digite seus novos dados------------ \n");
                  
                  fflush(stdin);
            printf("\nDigite o nome completo: ");
            gets(ctt.nome);
            
            fflush(stdin);
            printf("Digite o sua matricula atual (inalterável): ");
            gets(ctt.matricula);
        
            fflush(stdin);
            printf("Digite o email: ");
            gets(ctt.email);
        
            fflush(stdin);
            printf("Digite o salario: ");
            gets(ctt.salario);
        
            printf("Digite a data de admissão (utilize o espaço)");
            scanf("%d %d %d", &ctt.admissao.dia, &ctt.admissao.mes, &ctt.admissao.ano);
            
            printf("-----------------------------------------------\n\n");
            
            fwrite(&ctt, sizeof(CONTATO), 1, arquivo);   //permite a escrita de um bloco no arquivo
            
            printf("\n\nDados do usuário alterados com sucesso!\n");
                }
            else
                {
                    remove("tmp.txt");
                }
        } 
    } 
   else {
      printf("\nA matricula não consta no sistema, volte ao menu e cadastre :D\n");
        printf("------------------------------------------------------------------------\n\n");
  }
        
    
      printf("Para voltar ao menu digite 0\n");
    fclose(temp);
      fclose(arquivo);
      getch();
  }
 }
  
  
void remover(){
  FILE* temp;
    FILE* arquivo;
  CONTATO ctt;
  char matricula3[30];
 
  arquivo = fopen("cadastro.txt","rb");    //abrir em modo rb leitura binaria
  temp = fopen("tmp.txt","wb");    //abrir em modo wb ele limpa e grava binario
 
  if(arquivo == NULL && temp == NULL){
  
      printf("Problemas na abertura do arquivo!\n");
      getch();
  }
   else{
   
      fflush(stdin);
      printf("Digite a matrícula para excluir: ");
      gets(matricula3);
      
      while(fread(&ctt,sizeof(CONTATO),1,arquivo)==1)
    {
        if(strcmp(matricula3,ctt.matricula)==0)
      {
          printf("\nPor gentileza, confirme seus dados:\n");
      
        fflush(stdin);
        printf("\nDigite o nome completo: ");
        gets(ctt.nome);
      
        fflush(stdin);
        printf("Digite o email: ");
        gets(ctt.email);
          
        fflush(stdin);
        printf("Digite o salario: ");
        gets(ctt.salario);
        
        printf("Digite a data de admissão (utilize o espaço)");
        scanf("%d %d %d", &ctt.admissao.dia, &ctt.admissao.mes, &ctt.admissao.ano);
              
        printf("-----------------------------------------------\n\n");
    
        }else{
          
         
        fwrite(&ctt,sizeof(CONTATO),1,temp);   //permite a escrita de um bloco no arquivo
      
        }
      }
  
  fclose(arquivo);   //fechar o arq
  fclose(temp);   //fechar o temp
  
    if(strcmp(matricula3,ctt.matricula)==0){
      
    fflush(stdin);
    printf("Deseja deletar (s/n)? ");
  
    if(getche()=='s'){
      
        if(remove("cadastro.txt")==0  &&  rename("tmp.txt","cadastro.txt")==0) //verifica se as operacoes foram realizadas
              
          // o remove apaga o nome de arquivo fornecido para que ele não seja mais acessível
          // o rename renomeia o arquivo, fazendo com que ocorra uma troca de nomes de arquivos
      {      
      
          printf("\nOperacao realizada com sucesso!\n");
          
      }else{
        
            remove("tmp.txt");     //remover o arquivo tmp se a condicao foi "n" na hora de deletar
        }
    }
      
    } else {
      printf("\nA matricula não consta no sistema, volte ao menu e cadastre :D\n");
        printf("------------------------------------------------------------------------\n\n");
  }
  
  
  printf("Para voltar ao menu digite 0\n");
  fclose(temp);     //fechar o arq
  fclose(arquivo); //fechar o temp
  getch();
    
  } 
} 



