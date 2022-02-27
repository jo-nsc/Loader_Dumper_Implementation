#include <stdio.h>
#include <stdlib.h>

int char_to_int(char c);
int endereco_to_int(char endereco[]);
int byte_to_int(char byte[]);
char int_to_char(int i);
int char_to_int(char c);
void int_to_byte(int int_byte, char byte[]);
void int_to_endereco(int int_addr, char endereco[]);
void loader(char arquivo[], int* int_addr, int* tamanho);
void dumper(char arquivo[], int int_addr, int tamanho);
void limpar_memoria();
void imprimir_memoria();

int memoria[4096];


#define MAX_SIZE 256
#define TRUE 1

int main(){
    int int_addr = 0, tamanho = 4096, teste;
    int a = 1;
    char *file = malloc(MAX_SIZE);
    printf("Bem vindo, NAO utilize o dumper antes do loader\n");
    while(1){
        printf("\n\n ---------- \n\n");
        printf("Insira 1 caso queira utilizar o loader\n");
        printf("Insira 2 caso queira utilizar o dumper\n");
        printf("Insira 3 caso queira limpar a memoria\n");
        printf("Insira 4 caso queira imprimir a memoria\n");
        printf("Insira 5 caso queira sair\n");
        scanf("%i", &teste);
        if (teste==1){
            printf("Digite o nome do arquivo do loader (inclusive a extensao):\n");
            scanf("%s", file);
            loader(file, &int_addr,&tamanho);
        } 
        else if (teste == 2){
            printf("Digite o nome do arquivo do dumper (inclusive a extensao):\n");
            scanf("%s", file);
            dumper(file, int_addr, tamanho);
            printf("O Dumper funcionou com sucesso!\n");

        }
        else if (teste == 3){
            limpar_memoria();
            printf("Memoria limpa com sucesso\n");
        }
        else if (teste == 4){
            imprimir_memoria();
            printf("\nMemoria impressa com sucesso\n");
        }
        else if(teste == 5){
            break;
        }
    }
    

    loader("teste_loader.txt", &int_addr, &tamanho);

    dumper("teste_dumper.txt", int_addr, tamanho);

    printf("O programa foi executado com sucesso!");
    return 0;
}



void loader(char arquivo[], int* int_addr, int* tamanho){
    char endereco[3], char_temp[2], byte[2];
    int int_byte, i = 0;
    FILE *file;
    file = fopen(arquivo, "r");
    if (file) {
        fscanf(file, "%s", endereco);
        *int_addr = endereco_to_int(endereco);
        while (fscanf(file, "%s", byte) == 1){
            int_byte = byte_to_int(byte);
            memoria[*int_addr + i] = int_byte;
            i++;
        }
        fclose(file);
        printf("O Loader funcionou com sucesso!\n");
    } else{
        printf("\nErro de arquivo\n");
    }

    *tamanho = i;
}

void dumper(char arquivo[], int int_addr, int tamanho){

    int i = 0, j = 0;
    char endereco[3], byte[2];
    FILE *file;
    file = fopen(arquivo, "w");
    if (file) {
        int_to_endereco(int_addr, endereco);
        fprintf(file, "%c%c%c\n", endereco[0], endereco[1], endereco[2]);
        while (i < tamanho){
            int_to_byte(memoria[int_addr + i], byte);
            fprintf(file, "%c%c ", byte[0], byte[1]);
            if (j> 0 && j%15 == 0){
                fprintf(file, "\n");
                j = -1;
            }
            i++;
            j++;
        }
        fclose(file);
    }
}

void limpar_memoria(){
    int i = 0;
    while(i < 4096){
        memoria[i] = 0;
        i++;
    }
}

void imprimir_memoria(){
    int i = 0, j = 0;
    char endereco[3], byte[2];
    int_to_endereco(0, endereco);
    printf("%c%c%c\n", endereco[0], endereco[1], endereco[2]);
    while (i < 4096){
        int_to_byte(memoria[i], byte);
        printf("%c%c ", byte[0], byte[1]);
        if (j> 0 && j%15 == 0){
            printf("\n");
            j = -1;
        }
        i++;
        j++;
    }
}

int endereco_to_int(char endereco[]){
    return 16*16*char_to_int(endereco[0]) + 16*char_to_int(endereco[1]) + char_to_int(endereco[2]);
}

int byte_to_int(char byte[]){
    return 16*char_to_int(byte[0]) + char_to_int(byte[1]);
}

void int_to_endereco(int int_addr, char endereco[]){

    endereco[2] = int_to_char(int_addr % 16);
    endereco[1] = int_to_char((int_addr/16) % 16);
    endereco[0] = int_to_char(((int_addr/16)/16) % 16);

}

void int_to_byte(int int_byte, char byte[]){
    byte[1] = int_to_char(int_byte % 16);
    byte[0] = int_to_char((int_byte/16) % 16);

}

char int_to_char(int i){
    char c;

    if (i == 0)
        c = '0';
    else if (i == 1)
        c = '1';
    else if (i == 2)
        c = '2';
    else if (i == 3)
        c = '3';
    else if (i == 4)
        c = '4';
    else if (i == 5)
        c = '5';
    else if (i == 6)
        c = '6';
    else if (i == 7)
        c = '7';
    else if (i == 8)
        c = '8';
    else if (i == 9)
        c = '9';
    else if (i == 10)
        c = 'A';
    else if (i == 11)
        c = 'B';
    else if (i == 12)
        c = 'C';
    else if (i == 13)
        c = 'D';
    else if (i == 14)
        c = 'E';
    else if (i == 15)
        c = 'F';
    return c; 
}


int char_to_int(char c){

    int decimal;

    if (c == '0')
        decimal = 0;
    else if (c == '1')
        decimal = 1;
    else if (c == '2')
        decimal = 2;
    else if (c == '3')
        decimal = 3;
    else if (c == '4')
        decimal = 4;
    else if (c == '5')
        decimal = 5;
    else if (c == '6')
        decimal = 6;
    else if (c == '7')
        decimal = 7;
    else if (c == '8')
        decimal = 8;
    else if (c == '9')
        decimal = 9;
    else if (c == 'A' || c =='a')
        decimal = 10;
    else if (c == 'B' || c =='b')
        decimal = 11;
    else if (c == 'C' || c =='c')
        decimal = 12;
    else if (c == 'D' || c =='d')
        decimal = 13;
    else if (c == 'E' || c =='e')
        decimal = 14;
    else if (c == 'F' || c =='f')
        decimal = 15;
    else
        decimal = 99999;
    return decimal;
}

