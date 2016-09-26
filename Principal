#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define TRUE 1
#define FALSE 0
#define MAX 100
#define TRIP_ID 9
#define TRIP_FILE "viagens"    /*Inserir localização do ficheiro "viagens"*/
#define CLIENT_FILE "clientes" /*Inserir localização do ficheiro "clientes"*/

typedef struct client_node *Client_list;        /*Client database*/
typedef struct trip_node *Trip_list;            /*Trip database*/

typedef struct{
    int ano, mes, dia, hora, minuto; /*remove some if needed*/
}Date;

typedef struct info_client{
    char name[MAX];      /*name of the client*/
    int id;             /*ID number*/
    Trip_list trips;     /*trips aquired by the client*/
}Client;

typedef struct info_trip{
    char destination[MAX], id[MAX];     /*destination and flight ID*/
    int seats_capacity;                         /*Number of seats (total)*/
    Date date;                                  /*trip date*/
    Client_list current_list, waiting_list;     /*List of the clients in the waiting list, "in the trip already"*/
}Trip;

typedef struct client_node{
    Client info_client;
    Client_list next;
}List_client_node;

typedef struct trip_node{
    Trip info_trip;
    Trip_list next;
}List_trip_node;

void limpa_buffer(){        /*Função Utilizada para limpar o buffer (ou seja, remover tudo o que não irá ser preciso)*/
    char caracter;
    do{
        caracter = getchar();
    }while(caracter != '\n' && caracter != EOF);
}

int cartaocidadao(){
    int cc;
    char term;

    printf("Por favor introduza o Número de Identificação:\n\n");
    do{
        printf("-> ");
        scanf("%d%c", &cc, &term);
        if(cc < 10000000 || cc > 99999999 || term != '\n'){
            printf("Numero inválido! \n\nIntroduza novamente:\n\n");
        }
        if(term != '\n'){
            limpa_buffer();
        }
    }while(cc < 10000000 || cc > 99999999 || term != '\n');
    printf("\nNumero aceite!\n\n");
    return cc;
}

Client_list create_client_list(){            /*initializes the client list*/
    Client_list clients;
    clients = (Client_list)malloc(sizeof(List_client_node));
    if(clients != NULL){
        clients->info_client.id = 0;
        clients->next = NULL;
    }
    return clients;
}

Trip_list create_trip_list(){                    /*initializes the trips list*/
    Trip_list trips;
    trips = (Trip_list)malloc(sizeof(List_trip_node));
    if(trips != NULL){
        trips->next = NULL;
    }
    return trips;
}

void destroy_client_list(Client_list clients){
    Client_list aux;
    while(clients != NULL){
        aux = clients;
        clients = clients->next;
        free(aux);
    }
}

void destroy_trip_list(Trip_list trips){
    Trip_list aux;
    while(trips != NULL){
        aux = trips;
        trips = trips->next;
        free(aux);
    }
}

Date input_date(){          
    Date data;

    do{
        printf("\n\nAno ->");
        scanf("%d", &(data.ano));

        if(data.ano < 0){
            printf("Ano invalido, insira novamente\n");
        }
    }while(data.ano < 1);

    do{
        printf("\n\nMes ->");
        scanf("%d", &(data.mes));

        if(data.mes < 0){
            printf("Mes invalido, insira novamente\n");
        }
    }while(data.mes < 1 || data.mes >12);

    do{
        printf("\n\nDia ->");
        scanf("%d", &(data.dia));
        if(data.mes == 2 && (data.ano-2000)%4 == 0){
            if(data.dia < 0 || data.dia > 29){
                printf("Dia inválido, insira novamente\n");
            }
        }
        else if(data.mes == 2 && (data.ano-2000)%4 != 0){
            if(data.dia < 0 || data.dia > 28){
                printf("Dia inválido, insira novamente\n");
            }
        }
        else if(data.mes ==  1 ||data.mes ==  3 ||data.mes ==  5 ||data.mes ==  7 ||data.mes ==  8 ||data.mes ==  10 ||data.mes ==  12){
            if(data.dia < 0 || data.dia > 31){
                printf("Dia inválido, insira novamente\n");
            }
        }
        else if(data.mes ==  4 || data.mes ==  6 || data.mes ==  9 || data.mes ==  11){
            if(data.dia < 0 || data.dia > 30){
                printf("Dia inválido, insira novamente\n");
            }
        }
        else{
            break;
        }
    }while(data.dia < 1 || (data.dia > 31 && (data.mes ==  1 ||data.mes ==  3 ||data.mes ==  5 ||data.mes ==  7 ||data.mes ==  8 ||data.mes ==  10 ||data.mes ==  12)) || (data.dia > 30 && (data.mes ==  4 || data.mes ==  6 || data.mes ==  9 || data.mes ==  11)) || (data.dia > 29 && (data.mes == 2 && (data.ano-2000)%4 == 0)) || (data.dia > 28 && (data.mes == 2 && (data.ano-2000)%4 != 0)));

    do{
        printf("\n\nHora ->");
        scanf("%d", &(data.hora));

        if(data.hora < 0){
            printf("Hora inválido, insira novamente\n");
        }
    }while(data.hora < 0 || data.hora >23);

    do{
        printf("\n\nMinutos ->");
        scanf("%d", &(data.minuto));

        if(data.minuto < 0){
            printf("Minutos inválido, insira novamente\n");
        }
    }while(data.minuto < 0 || data.minuto >59);
    return data;
}

void add_client(Client_list header, Client info_client){        /*adds a new client to the database(list) --> Check first if not already in <--*/
    Client_list new_client, client;
    client = header;
    while(client->next != NULL){    /*goes to the end of the list*/
        client = client->next;
    }

    new_client = (Client_list)malloc(sizeof(List_client_node));

    if(new_client != NULL){
        client->next = new_client;  /*adds to the end of the current list*/
        new_client->next = NULL;    /*defines the new end of the list*/
        new_client->info_client = info_client;
        new_client->info_client.trips = NULL;
    }
}

void add_trip(Trip_list trips, Trip info_trip){
    Trip_list new_trip, current;
    current = trips;

    while(current->next != NULL){ /*end of the list*/
        current = current->next;
    }

    new_trip = (Trip_list)malloc(sizeof(List_trip_node));

    if(new_trip != NULL){
        current->next = new_trip;
        new_trip->info_trip = info_trip;
        new_trip->next = NULL;
    }
}

int client_in_database(Client_list clients, int id) {      /*compares the given id with the ones in the database, if doesnt exit, adds a new node to the client list*/
    Client_list list = clients;
    while(list != NULL){
        /*comprarar id fornecido a funçao com os ids da lista*/
        if(list->info_client.id == id){     /*Se existir*/
            return 0;
        }
        list = list->next;
    }
    return 1;    /*existe-> 0 | não existe->1*/
}

void imprime_data(Date d){
    printf("%d/%d/%d %.2d:%.2d", d.dia, d.mes, d.ano, d.hora, d.minuto);
}

void imprime_nome_cliente(Client_list clients, int id){
    Client_list current;
    current = clients->next;

    while(current != NULL && current->info_client.id != id){
        current = current->next;
    }

    printf("%s\n", current->info_client.name);
}

void imprime_lista_viagens_do_cliente(Trip_list trips, Client_list clients, int id){
    Trip_list current_trip;

    current_trip = trips;
    /*system("clear");*/
    printf("Lista de viagens de:  ");
    imprime_nome_cliente(clients, id);
    printf("Data - Destino\nCodigo\n\n");
    while(current_trip != NULL){
        /*"dia/mes/ano hora:minutos - Destino:
        Codigo*/
        imprime_data(current_trip->info_trip.date);
        printf(" - %s%s\n", current_trip->info_trip.destination, current_trip->info_trip.id);
        current_trip = current_trip->next;
    }
}

int available_seats(Trip current){    /*counts the number of available seats in one trip*/
    int available;
    Client_list aux;

    aux = current.current_list;
    available = current.seats_capacity;

    while(aux != NULL){     /*Current list é a lista dos clientes da viagem!*/
        if(aux->info_client.id != 0){
            available--;
        }aux = aux->next;
    }
    return available;
}

char remove_espacos(char caracter){         /*Remover espaços em branco do inicio do input(Basically)*/
    char current_char = caracter;           /*Mudar nome da variavel*/
    while(current_char == ' '){             /*Remove espaços*/
        current_char = getchar();
    }
    return current_char;
}

int pede_cc(){
    int cc = 0;
    char confirm, term;

    /*numero do cc do cliente*/
    while (TRUE){
        cc = cartaocidadao();
        /*system("clear");*/
        printf("<%d> E o seu cartao do cidadao?\n", cc);
        printf("Em caso afirmativo, escreva 's', em caso negativo, escreva 'n'\n\n");

        do{
            printf("-> "); 
            confirm = getchar(); 
            if(confirm != '\n'){
                term = getchar();
                if(term != '\n'){
                    confirm = '0';
                    limpa_buffer();
                    continue;
                }
            }
        }while(confirm != 's' && confirm != 'S' && confirm != 'n' && confirm != 'N');

        if (confirm == 's' || confirm == 'S'){
            break;
        }
        /*
        else if(confirm == 'n' || confirm == 'N'){ 
            continue;
        }
        */
    }
    return cc;
}

void viagens_do_cliente(Trip_list trips, Client_list clients,int client_id){
    Trip_list current_trip, aux_trip, client_trip;                           /*aux_trip used to create the new trip thingy*/
    Client_list current_client, aux_search, aux_search_wait;        /*Client_trip == used to add the trip info to the new list*/

    current_trip = trips;
    current_client = clients;

    /*Encontra node do cliente*/
    while(current_client != NULL && current_client->info_client.id != client_id){
        current_client = current_client->next;
    }
    if(current_client->info_client.trips != NULL){
        current_client->info_client.trips = NULL;
    }

    /*Scans all trips, for the client id*/
    while(current_trip != NULL){
        aux_search = current_trip->info_trip.current_list;                                              /*aux_search = "main" list*/
        aux_search_wait = current_trip->info_trip.waiting_list;                                         /*aux_search_wait = waiting list*/
        while(aux_search != NULL){                                                                      /*Main list search start*/
            if(client_id == aux_search->info_client.id){                                                /*if client is in the "main" list*/
                /*adds the trip to list of trips acquired by the client*/
                client_trip = (Trip_list)malloc(sizeof(List_trip_node));                                /*allocates memory*/
                if(client_trip != NULL){                                                                /*Se conseguir alocar memória*/
                    client_trip->info_trip = current_trip->info_trip;
                    client_trip->next = NULL;                                                           /*next = NULL, irá ser colocado no fim da lista*/
                }
                /*add new node to the list*/
                if(current_client->info_client.trips == NULL){                                          /*Se a lista de viagens do cliente estiver vazia*/
                    current_client->info_client.trips = client_trip;
                }
                else{                                                                                   /*Se a lista de viagens do cliente nao estiver vazia*/
                    aux_trip = current_client->info_client.trips;                                       /*aux_trip used to preserve (not lose) the "header" of this list*/
                    while(aux_trip->next != NULL){                                                      /*Vai para o fim da lista*/
                        aux_trip = aux_trip->next;
                    }
                    aux_trip->next = client_trip;
                }
                break;
            }
        if(aux_search != NULL && aux_search->next == NULL){
            while(aux_search_wait != NULL){                                                                 /*Waiting list search start*/
                if(client_id == aux_search_wait->info_client.id){                                           /*If client is in the waiting list*/
                    /*adds the trip to list of trips acquired by the client*/
                    client_trip = (Trip_list)malloc(sizeof(List_trip_node));                                /*allocates memory*/
                    if(client_trip != NULL){                                                                /*Se conseguir alocar memória*/
                        client_trip->info_trip = current_trip->info_trip;
                        client_trip->next = NULL;                                                           /*next = NULL, irá ser colocado no fim da lista*/
                    }
                    /*add new node to the list*/
                    if(current_client->info_client.trips == NULL){                                          /*Se a lista de viagens do cliente estiver vazia*/
                        current_client->info_client.trips = client_trip;
                    }
                    else{                                                                                   /*Se a lista de viagens do cliente nao estiver vazia*/
                        aux_trip = current_client->info_client.trips;                                       /*aux_trip used to preserve (not lose) the "header" of this list*/
                        while(aux_trip->next != NULL){                                                      /*Vai para o fim da lista*/
                            aux_trip = aux_trip->next;
                        }
                        aux_trip->next = client_trip;
                    }
                    break;
                }
                aux_search_wait = aux_search_wait->next;
            }
        }
        aux_search = aux_search->next;
        }
        current_trip = current_trip->next;
    }
    imprime_lista_viagens_do_cliente(current_client->info_client.trips, clients, client_id);
    /*limpa_buffer();*/
}

int destino_na_lista(Trip_list trips, char *destino){
    Trip_list current;

    current = trips;
    while(current != NULL){
        if(strncmp(destino, current->info_trip.destination, 4) == 0){
            return 1;
        }
        current = current->next;
    }
    return 0;
}
/*
int compara_datas(Date *d1, Date *d2){
    if((d1 -> ano) > (d2 -> ano)){
        return 1;
    }
    else if((d1 -> ano) < (d2 -> ano)){
        return -1;
    }
    else{
        if((d1 -> mes) > (d2 -> mes)){
            return 1;
        }
        else if((d1 -> mes) < (d2 -> mes)){
            return -1;
        }
        else{
            if((d1 -> dia) > (d2 -> dia)){
                return 1;
            }
            else if((d1 -> dia) < (d2 -> dia)){
                return 1;
            }
            else{
                if((d1 -> hora) > (d2 -> hora)){
                    return 1;
                }
                else if((d1 -> hora) < (d2 -> hora)){
                    return -1;
                }
                else{
                    if((d1 -> minuto) > (d2 -> minuto)){
                        return 1;
                    }
                    else if((d1 -> minuto) < (d2 -> minuto)){
                        return -1;
                    }
                }
            }
        }
    }
    return 0;
}*/

void listar_destinos(Trip_list trips){
    Trip_list current, aux_destinos, current_aux;
    int flag;   /*1 se ainda não existe, 0 se já existe*/

    aux_destinos = create_trip_list();
    current = trips->next;

    while(current != NULL){
        flag = 1;
        current_aux = aux_destinos->next;
        while(current_aux != NULL){
            if(strcmp(current->info_trip.destination, current_aux->info_trip.destination) == 0){
                flag = 0;
            }
            current_aux = current_aux->next;
        }
        if(flag){
            add_trip(aux_destinos,current->info_trip);
        }
        current = current->next;
    }
    current_aux = aux_destinos->next;
    /*system("clear");*/
    printf("Destinos disponiveis:\n\n");
    while(current_aux != NULL){
        printf("%s\n",current_aux->info_trip.destination);
        current_aux = current_aux->next;
    }
    destroy_trip_list(aux_destinos);
}

void Cliente_na_viagem(Trip_list trip, int id){
    /*Percorrer a lista a ver se o cc do utilizador já existe na mesma*/
    Trip_list current;
    Client_list current_c;
    current = trip;
    current_c = current->info_trip.current_list;
    while(current_c != NULL){
        if(current_c->info_client.id == id){
            printf("\nCliente já reservou um lugar nesta viagem.\n");
            return;
        }
        current_c = current_c->next;
    }
    current_c = current->info_trip.waiting_list;
    while(current_c != NULL){
        if(current_c->info_client.id == id){
            printf("\nCliente já reservou um lugar nesta viagem, mas encontra-se em fila de espera.\n");
            return;
        }
        current_c = current_c->next;
    }
}

void lista_viagens_destino(Trip_list trips, char *destino, int id){
    Trip_list current, new_list, new_trip, aux;
    int vagas;
    current = trips;
    new_list = create_trip_list();
    while(current != NULL){
        if(strncmp(destino, current->info_trip.destination, 4) == 0){
            new_trip = (Trip_list)malloc(sizeof(List_trip_node));
            if(new_trip != NULL){
                new_trip->info_trip = current->info_trip;
                aux = new_list->next;
                new_trip->next = aux;
                new_list->next = new_trip;
            }
        }
        current = current->next;
    }
    new_trip = new_list->next;
    while(new_trip != NULL){
        vagas = available_seats(new_trip->info_trip);
        imprime_data(new_trip->info_trip.date);
        printf(" - %svagas(%d/%d)",new_trip->info_trip.id, vagas, new_trip->info_trip.seats_capacity);
        Cliente_na_viagem(new_trip, id);
        printf("\n\n");
        new_trip = new_trip->next;
    }
    destroy_trip_list(new_list);
}

void reserva(Trip_list trips, int id, char *viagem, char *destino){
    Trip_list current;
    Client_list client, aux_lista, aux_espera;

    current = trips;

    while(current != NULL && strncmp(current->info_trip.id , viagem, 8) != 0){
        current = current->next;
    }

    if(current == NULL){
        limpa_buffer();
        /*system("clear");*/
        printf("Codigo de viagem introduzido nao encontrado, por favor insira novamente!\n\n");
        printf("Viagens Disponiveis:\n\n");
        lista_viagens_destino(trips, destino, id);
        do{
            printf("-> ");
            fgets(viagem, TRIP_ID, stdin);
        }while(strlen(viagem) != 8);
        reserva(trips, id, viagem, destino);
        return;
    }

    client = (Client_list)malloc(sizeof(List_client_node));

    if(client != NULL){
        client->info_client.id = id;
        client->next  = NULL;
    }

    aux_lista = current->info_trip.current_list;
    aux_espera = current->info_trip.waiting_list;


    if(available_seats(current->info_trip)){       /*Se houver lugar*/
        while(aux_lista->next != NULL){
            aux_lista = aux_lista->next;
        }
        aux_lista->next = client;
    }
    else{                               /*Se NÃO houver lugar*/
        while(aux_espera->next != NULL){
            aux_espera = aux_espera->next;
        }
        aux_espera->next = client;
    }
}

void adquire_viagem(Trip_list trips, Client_list clients){
    char destino[MAX], id_viagem[MAX], confirm, nome[MAX], term;
    int cc, i = 0;
    Client info_client;
    /*dados do Cliente*/
    cc = pede_cc();        /*numero do CC*/
    info_client.id = cc;
    /*system("clear");*/
    if(client_in_database(clients, cc)){   /*se o cliente não estiver na base, retorna (1) e acidiona o cliente*/
        printf("Nome do cliente:\n-> ");
        /*fgets(info_client.name, MAX, stdin); */          /*pede para introduzir o nome do cliente*/
        limpa_buffer();
        while(i < MAX-1){
            nome[i] = getchar();
            if(nome[i] == '\n'){
                info_client.name[i+1] = '\0';
                break;
            }
            i++;
        }
        strcpy(info_client.name, nome);
        add_client(clients, info_client);
    }
    else{
        printf("cliente: ");
        imprime_nome_cliente(clients, cc);
        printf("Em caso afirmativo, escreva 's', em caso negativo, escreva 'n'\n\n");
        do{
            /*se for o cliente "impresso", continua, caso contrário volta a pedir o cc*/
            printf("-> ");
            /*limpa_buffer();*/
            confirm = getchar();
            if(confirm != '\n'){
                term = getchar();
                if(term != '\n'){
                    limpa_buffer();
                    continue;
                }
            }
            
            if (confirm == 's' || confirm == 'S'){
                break;
            }
            else if(confirm == 'n' || confirm == 'N'){
                adquire_viagem(trips, clients);
                return;
            }
        }while(TRUE);
    }
    /*escolher destino*/
    do{
        /*printf("Escolha o destino:\n");*/
        /*Função para listar destinos*/
        listar_destinos(trips);
        printf("-> ");
        fgets(destino, MAX, stdin);
        if(destino_na_lista(trips, destino)){
            break;
        }
    }while(TRUE);
    /*system("clear");*/
    /*Listar viagens: com destino selecionado anteriormente*/
    /*Formato: "Dia/Mes/Ano Hora:Minutos - Código_da_viagem"*/
    lista_viagens_destino(trips, destino, cc);
    printf("Introduza o ID da viagem:\n");
    /*Introduz o ID da viagem*/
    do{
        printf("-> ");
        fgets(id_viagem, TRIP_ID, stdin); 
        /*if(strlen(id_viagem ))*/
    }while(strlen(id_viagem) != 8); /*Proteger de maneira a ver se o codigo inserido existe*/
    reserva(trips, cc, id_viagem, destino); /*Viagem = node onde se efetua a reserva, se viagem estiver cheia, correr adquire_viagem_reserva*/
    printf("\n\nViagem reservada com sucesso!\n");
}

Client encontra_node(Client_list clients,int cc){
    Client_list current;
    current = clients;
    while(current->info_client.id != cc){
        current = current->next;
    }
    return current->info_client;
}

void cancela(Trip_list trips, int cc, Client_list clients, char *viagem){
    Client_list current_client, previous, aux, aux2;
    Trip_list current;
    current = trips;
    /*encontrar a viagem*/
    while(current != NULL && strncmp(viagem, current->info_trip.id, 8) != 0){
        current = current->next;
    }

    if(current == NULL){
        limpa_buffer();
        printf("\n\n\n\nCodigo de viagem introduzido não encontrado, por favor insira novamente!\n");
        viagens_do_cliente(trips, clients, cc);
        printf("-> ");
        fgets(viagem, TRIP_ID, stdin);
        cancela(trips, cc, clients, viagem);
        return;
    }

    current_client = current->info_trip.current_list;

    while(current_client != NULL){
        if(current_client->info_client.id == cc){
            previous->next = current_client->next;
            free(current_client);
            aux2 = previous;
            if(current->info_trip.waiting_list != NULL){
                aux = current->info_trip.waiting_list->next;
                previous = current->info_trip.waiting_list;
                while(aux2->next != NULL){
                    aux2 = aux2->next;
                }
                previous->next = aux->next;
                aux->next = aux2->next;
                aux2->next = aux;
                return;
            }
        }
        previous = current_client;
        current_client = current_client->next;
      }
        /*se o cliente se encontra em fila de espera, repete o que está acima*/
    current_client = current->info_trip.waiting_list;
    if(current_client->info_client.id == cc){
        previous = current_client->next;
        current->info_trip.current_list = previous;
        free(current_client);
        return;
    }
    else{
        while(current_client != NULL){
            previous = current_client;
            current_client = current_client->next;
            if(current_client->info_client.id == cc){
                previous->next = current_client->next;
                free(current_client);
                return;
            }
        }
    }
    printf("O cliente nao se encontra na viagem selecionada. Verifique se escreveu bem o codigo.\n");
}

void cancela_reserva(Trip_list trips, Client_list clients){
    char *trip_id, trip_cenas[8];
    Client info_client;
    trip_id = trip_cenas;

    /*Pedir o cc ao cliente*/
    info_client.id = pede_cc();

    /*Listar viagens do cliente*/
    viagens_do_cliente(trips, clients, info_client.id);
    /*Escolher ID da viagem a cancelar*/
    printf("Insira o Codigo da viagem a cancelar:\n\n");
    do{
        printf("-> ");
        fgets(trip_id, TRIP_ID, stdin);              /*Mudar o numero de caracteres do trip_id*/
    }while(strlen(trip_id)  != 8);
    cancela(trips, info_client.id, clients, trip_id);
}

void list_all_clients(Client_list clients){
    Client_list current;
    current = clients->next;
    printf("Lista de clientes:\n\n");
    while(current != NULL){
        printf("cc: %d - %s\n", current->info_client.id, current->info_client.name);
        current = current->next;
    }
}

void read_clients(Client_list clients, char *ficheiro){
    FILE *fp;
    Client client;
    fp = fopen(ficheiro, "r");
    if(fp != NULL){
        while(fgets(client.name, MAX, fp) != NULL){
            fscanf(fp, "%d\n", &client.id);
            add_client(clients, client);
        }
        fclose(fp);
    }
}

void write_clients(Client_list clients, char *ficheiro){
    FILE *fp;
    Client_list current;
    current = clients->next;
    fp = fopen(ficheiro, "w");

    if(fp != NULL){
        while(current != NULL){
            fprintf(fp, "%s%d\n", current->info_client.name, current->info_client.id);
            current = current->next;
        }
    }
    fclose(fp);
}

void read_viagens(Trip_list trips, char *ficheiro){
    FILE *fp;
    Trip trip;
    Client_list viagem, espera;
    Client client;
    char temp[MAX];
    int cc;
    fp = fopen(ficheiro, "r");
    if(fp != NULL){
        while(fgets(temp, MAX, fp) != NULL){    /*enquanto nao chega ao fim do ficheiro*/
            if(strncmp("VIAGEM", temp,6) == 0){
                fgets(trip.id, MAX, fp);
                viagem = create_client_list();
                fgets(trip.destination, MAX, fp);
                fscanf(fp, "%d:%d-%d/%d/%d*%d\n", &trip.date.hora, &trip.date.minuto, &trip.date.dia, &trip.date.mes, &trip.date.ano, &trip.seats_capacity);
                fgets(temp, MAX, fp);   /*remove a linha "LUGARES"*/
                do{
                    fgets(temp, MAX, fp);
                    cc = atoi(temp);
                    client.id = cc;
                    add_client(viagem, client);
                }while(cc != 0);

                espera = create_client_list();

                do{
                    fgets(temp, MAX, fp);
                    cc = atoi(temp);
                    client.id = cc;
                    add_client(espera, client);
                }while(cc != 0);
                trip.current_list = viagem;
                trip.waiting_list = espera;
                add_trip(trips, trip);
            }
        }
        fclose(fp);
    }
}

void write_viagens(Trip_list trips, char *ficheiro){
    FILE *fp;
    Trip_list current;
    Client_list aux;
    current = trips->next;
    fp = fopen(ficheiro, "w");

    if(fp != NULL){
        while(current != NULL){
            fprintf(fp, "VIAGEM\n%s%s%d:%.2d-%.2d/%.2d/%d*%d\nLUGARES\n", current->info_trip.id ,current->info_trip.destination, current->info_trip.date.hora, current->info_trip.date.minuto, current->info_trip.date.dia, current->info_trip.date.mes, current->info_trip.date.ano, current->info_trip.seats_capacity);
            aux = current->info_trip.current_list;
            while(aux != NULL){
                if(aux->info_client.id == 0){
                    aux = aux->next;
                    continue;
                }
                fprintf(fp, "%d\n", aux->info_client.id);
                aux = aux->next;
            }
            aux = current->info_trip.waiting_list;
            fprintf(fp, "ESPERA\n");
            while(aux != NULL){
                if(aux->info_client.id == 0){
                    aux = aux->next;
                    continue;
                }
                fprintf(fp, "%d\n", aux->info_client.id);
                aux = aux->next;
            }
            fprintf(fp, "END\n");
            current = current->next;
        }
    }
    fclose(fp);
}

void menu(Trip_list trips, Client_list clients){
    /*declarar variaveis*/
    char opcao, sec_elem, destino[MAX];
    /*Lista de opções numerada*/
    printf("Que deseja fazer?\n\n"); /*Linha que aparece antes do "menu"*/
    printf("1) Reservar Viagem\n");
    printf("2) Cancelar Reserva\n");
    printf("3) Listar todas as viagens para o destino\n");
    printf("4) Listar todas as viagens adquiridas pelo cliente\n");
    printf("5) Lista de todos os clientes\n");
    printf("0) Sair do programa\n");
    printf("\n");

    /*escolher o numero*/
    do{
        printf("-> ");  /*editar o texto do printf*/
        opcao = getchar();      /*primeiro caracter inserido*/
        if(opcao == ' '){
            opcao = remove_espacos(opcao);
        }

        else if(opcao == '\n'){      /*caso se introduza apenas '\n', reinicia o ciclo*/
            continue;
        }

        sec_elem = getchar();   /*vai buscar o segundo elemento (supostamente '\n')*/

        if(sec_elem == ' '){
            sec_elem = remove_espacos(sec_elem);
        }

        if(opcao >= '0' && opcao <= '5' && sec_elem == '\n'){
            break;
        }

        else{
            printf("\n\nInput inválido, insira novamente:\n");
            if(sec_elem != '\n'){
                limpa_buffer();
            }
        }

    }while(TRUE);

    /*system("clear");*/
    /*opções (case)*/
    switch(opcao){
        case '1':
            /*adquirir viagem / lista de espera*/
            adquire_viagem(trips, clients);
            limpa_buffer();
            break;

        case '2':
            /*cancelar viagem / lista de espera*/
            cancela_reserva(trips, clients);
            limpa_buffer();
            break;

        case '3':
            /*listar todas as viagens para um determinado destino*/

            do{
                listar_destinos(trips);
                printf("-> ");
                fgets(destino, MAX, stdin);
                if(destino_na_lista(trips, destino)){
                    break;
                }
            }while(TRUE);
            /*system("clear");*/
            lista_viagens_destino(trips, destino, 00000001);
            break;

        case '4':
            /*Viagens adquiridas pelo cliente*/
            viagens_do_cliente(trips, clients, pede_cc());
            break;

        case '5':
            /*listar todos os clientes (sem repetição)*/
            list_all_clients(clients);
            break;

        default:    /*case '0'*/
            write_clients(clients, CLIENT_FILE);
            write_viagens(trips, TRIP_FILE);
            printf("Obrigado! \n");
            exit(0);
    }
    write_clients(clients, CLIENT_FILE);
    write_viagens(trips, TRIP_FILE);
}

void programa(Trip_list trips, Client_list clients){
    char yes_no, sec_elem;
    do{
        /*system("clear");*/
        menu(trips, clients);

        do{
            printf("\n\nRealizar mais alguma operação?\nSim - 1 |Nao - 0\n");
            printf("-> ");
            yes_no = getchar();

            if(yes_no == ' '){
                yes_no = remove_espacos(yes_no);
            }

            if(yes_no == '\n'){
                continue;
            }

            sec_elem = getchar();

            if(sec_elem == ' '){
                sec_elem = remove_espacos(sec_elem);
            }

            if(yes_no >= '0' && yes_no <= '1' && sec_elem == '\n'){
                break;
            }

            else{
                printf("\n\nERRO: Input inválido.");
                if(sec_elem != '\n'){
                    limpa_buffer();
                }
            }
        }while(TRUE);

        if (yes_no == '0'){   /*Se o input for '0', termina o programa*/
            return;
        }

        printf("\n\n");     /*Para o texto na consola não ficar tão compacto, e ficar mais agradavel a vista*/
    }while(TRUE);           /*Enquanto TRUE, corre o loop*/
}

int main(){
    Client_list clients = create_client_list();
    Trip_list trips = create_trip_list();

    read_clients(clients, CLIENT_FILE);
    read_viagens(trips, TRIP_FILE);

    programa(trips, clients); /*Inicia a função "principal" do programa*/

    write_clients(clients, CLIENT_FILE);
    write_viagens(trips, TRIP_FILE);
    return 0;
}
