/ Includes para opera??es de entrada/sa?da e manipula??o de strings
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

#define MAX 100 // capacidade m?xima do array de containers

// ================= STRUCT =================
// Estrutura que representa um container com propriedades b?sicas
struct Container {
    int id;           // identificador do container
    float peso;       // peso em toneladas (ou unidade definida)
    float volume;     // volume em m^3 (ou unidade definida)
    string origem;    // local de origem
    string destino;   // local de destino
};

// ================= VARIAVEIS =================
// Vetor est?tico para armazenar os containers cadastrados
Container containers[MAX];
// Quantidade atual de containers cadastrados
int total = 0;

// ================= FUNCOES =================

// Cadastro
// L? os dados de um novo container via entrada padr?o e adiciona ao vetor
void cadastrar() {
    if (total >= MAX) {
        cout << "Limite de containers atingido!\n";
        return;
    }

    Container c;

    // Leitura dos campos do container
    cout << "\nID: ";
    cin >> c.id;

    cout << "Peso: ";
    cin >> c.peso;

    cout << "Volume: ";
    cin >> c.volume;

   cout << "Origem: ";
cin.ignore();
getline(cin, c.origem);

cout << "Destino: ";
getline(cin, c.destino);

    // Armazena no array e incrementa o total
    containers[total++] = c;

    cout << "Container cadastrado!\n";
}

// Listar
// Exibe todos os containers cadastrados com suas propriedades
void listar() {
    cout << "\n=== LISTA DE CONTAINERS ===\n";

    if (total == 0) {
        cout << "Nenhum container cadastrado.\n";
        return;
    }

    for (int i = 0; i < total; i++) {
        cout << "ID:" << containers[i].id
            << " | Peso:" << containers[i].peso
            << " | Volume:" << containers[i].volume
            << " | " << containers[i].origem
            << " -> " << containers[i].destino << endl;
    }
}

// Ordenar por peso
// Implementa um bubble sort simples para ordenar o array pelo campo 'peso'
void ordenarPeso() {
    for (int i = 0; i < total - 1; i++) {
        for (int j = 0; j < total - i - 1; j++) {
            if (containers[j].peso > containers[j + 1].peso) {
                Container temp = containers[j];
                containers[j] = containers[j + 1];
                containers[j + 1] = temp;
            }
        }
    }
    cout << "Ordenado por peso!\n";
}

// Ordenar por volume
// Ordena o array pelo campo 'volume' usando bubble sort
void ordenarVolume() {
    for (int i = 0; i < total - 1; i++) {
        for (int j = 0; j < total - i - 1; j++) {
            if (containers[j].volume > containers[j + 1].volume) {
                Container temp = containers[j];
                containers[j] = containers[j + 1];
                containers[j + 1] = temp;
            }
        }
    }
    cout << "Ordenado por volume!\n";
}

// Simulacao
// Calcula custo e tempo estimados para cada container numa rota de dada distancia
void calcularRota() {
    int distancia;
    float custo, tempo;

    cout << "\nDistancia da rota (km): ";
    cin >> distancia;

    if (total == 0) {
        cout << "Nenhum container cadastrado.\n";
        return;
    }

    for (int i = 0; i < total; i++) {
        // F?rmula simples de exemplo para custo e tempo
        custo = containers[i].peso * 0.5f + distancia * 0.2f;
        tempo = distancia / 60.0f; // assume velocidade m?dia 60 km/h

        cout << "\nContainer " << containers[i].id << ":";
        cout << "\nCusto estimado: " << custo;
        cout << "\nTempo estimado: " << tempo << " horas\n";
    }
}

// Salvar (2 arquivos)
// Persiste os dados em um arquivo simples para carregar depois e gera um relat?rio leg?vel
void salvar() {
    // Arquivo para o sistema (simples)
    ofstream f("containers.txt");

    if (!f) {
        cout << "Erro ao salvar!\n";
        return;
    }

    for (int i = 0; i < total; i++) {
        // Grava em formato: id peso volume origem destino
        f << containers[i].id << " "
            << containers[i].peso << " "
            << containers[i].volume << " "
            << containers[i].origem << " "
            << containers[i].destino << endl;
    }

    f.close();

    // Arquivo bonito (relatorio)
    ofstream r("relatorio.txt");

    if (!r) {
        cout << "Erro ao gerar relatorio!\n";
        return;
    }

    r << "===== RELATORIO DE CONTAINERS =====\n\n";

    for (int i = 0; i < total; i++) {
        r << "===== CONTAINER =====\n";
        r << "ID: " << containers[i].id << "\n";
        r << "Peso: " << containers[i].peso << "\n";
        r << "Volume: " << containers[i].volume << "\n";
        r << "Origem: " << containers[i].origem << "\n";
        r << "Destino: " << containers[i].destino << "\n";
        r << "---------------------\n\n";
    }

    r.close();

    cout << "Dados salvos e relatorio gerado!\n";
}

// Carregar (usa arquivo simples)
// L? o arquivo gerado por salvar() e preenche o vetor at? o limite MAX
void carregar() {
    ifstream f("containers.txt");

    if (!f) return; // se n?o existir o arquivo, apenas retorna

    while (f >> containers[total].id
        >> containers[total].peso
        >> containers[total].volume
        >> containers[total].origem
        >> containers[total].destino) {

        if (total < MAX) {
            total++;
        }
        else {
            break; // evita overflow do array
        }
    }

    f.close();
}

// ================= MENU =================
// Exibe menu principal e chama as fun??es conforme a escolha do usu?rio
void menu() {
    int op;

    do {
        cout << "\n===== SISTEMA DE ROTEAMENTO =====\n";
        cout << "1. Cadastrar container\n";
        cout << "2. Listar containers\n";
        cout << "3. Ordenar por peso\n";
        cout << "4. Ordenar por volume\n";
        cout << "5. Simular rota\n";
        cout << "6. Salvar dados\n";
        cout << "0. Sair\n";
        cout << "Escolha: ";
        cin >> op;

        switch (op) {
        case 1: cadastrar(); break;
        case 2: listar(); break;
        case 3: ordenarPeso(); break;
        case 4: ordenarVolume(); break;
        case 5: calcularRota(); break;
        case 6: salvar(); break;
        }

    } while (op != 0);
}

// ================= MAIN =================
int main() {
    // Ao iniciar, tenta carregar dados previamente salvos
    carregar();
    // Inicia o loop do menu principal
    menu();
    return 0;
}
