# Atividade-A3
package estudos;

import java.io.BufferedReader;
import java.io.File; // Importa a classe File para manipular arquivos e diretórios.
import java.io.FileInputStream; // Importa FileInputStream para ler bytes de um arquivo.
import java.io.FileOutputStream; // Importa FileOutputStream para escrever bytes em um arquivo.
import java.io.FileReader;
import java.io.FileWriter; // Importa FileWriter para escrever caracteres em um arquivo (usado para log).
import java.io.IOException; // Importa IOException para lidar com erros de entrada/saída.
import java.io.InputStream; // Importa InputStream, classe abstrata para fluxos de entrada de bytes.
import java.io.OutputStream; // Importa OutputStream, classe abstrata para fluxos de saída de bytes.
import java.io.PrintWriter; // Importa PrintWriter para escrever texto formatado em um arquivo.
import java.util.Scanner; // Importa Scanner para ler a entrada do usuário do console.

public class Atividade { // Declara a classe principal do programa.

    // Scanner estático para evitar múltiplas instâncias e garantir que seja fechado apenas uma vez.
    // 'static' significa que pertence à classe, não a um objeto.
    // 'private' significa que só pode ser acessado dentro desta classe.
    private static Scanner scanner = new Scanner(System.in);
    // Constante para o nome do arquivo de log.
    // 'final' significa que o valor não pode ser alterado após a inicialização.
    private static final String LOG_FILE = "log.txt";

    public static void main(String[] args) {
        /* Método principal, ponto de entrada do programa.
         * 'public' permite que seja acessado de fora.
         * 'static' permite que seja chamado sem criar um objeto da classe.
         * 'void' indica que não retorna nenhum valor.
         * 'String[] args' permite passar argumentos de linha de comando.
         */
        Atividade gerenciador = new Atividade();
        int opcao; // Variável para armazenar a opção escolhida pelo usuário.

        do {
            /* Inicia um loop 'do-while' que garante que o menu seja exibido pelo menos uma vez.
             * Exibe o menu e obtém a opção do usuário.
             */
            opcao = gerenciador.exibirMenu(); // Call the method on the 'gerenciador' instance.
            // Executa a operação correspondente à opção escolhida
            gerenciador.executarOpcao(opcao); // Call the method on the 'gerenciador' instance.
        } while (opcao != 9); // O loop continua até que o usuário escolha a opção "Sair".

        scanner.close(); // Fecha o objeto Scanner para liberar recursos do sistema.
    }

    /*
     * Exibe o menu de opções para o usuário e lê a escolha.
     * Garante que a entrada seja um número inteiro válido.
     * return A opção escolhida pelo usuário.
     */
    private int exibirMenu() { // Método privado para exibir o menu.
        System.out.println("\n--- MENU ---"); // Imprime o título do menu.
        System.out.println("[0] Abrir pasta");
        System.out.println("[1] Abrir arquivo");
        System.out.println("[2] Renomear arquivo");
        System.out.println("[3] Mover arquivo");
        System.out.println("[4] Copiar arquivo");
        System.out.println("[5] Excluir arquivo");
        System.out.println("[6] Criar arquivo");
        System.out.println("[7] Mostrar Uso de Memória"); // Nova opção para exibir o uso de memória.
        System.out.println("[8] Listar unidades de Disco");
        System.out.println("[9] Sair"); // Opção de saída atualizada.
        System.out.print("Escolha a opção: "); // Solicita a entrada do usuário.

        while (!scanner.hasNextInt()) { // Loop para validar a entrada: verifica se o próximo token é um inteiro.
            System.out.println("Entrada inválida. Por favor, digite um número."); // Mensagem de erro.
            scanner.next(); // Consome a entrada inválida para evitar loop infinito.
            System.out.print("Escolha a opção: "); // Pede a entrada novamente.
        }
        int opcao = scanner.nextInt(); // Lê o número inteiro digitado pelo usuário.
        scanner.nextLine(); // Consome a quebra de linha pendente após a leitura do inteiro,
        // para que futuras chamadas a nextLine() funcionem corretamente.
        return opcao; // Retorna a opção escolhida.
    }

    // Executa a operação correspondente à opção fornecida pelo usuário.
    // opcao A opção numérica escolhida pelo usuário.

    private void executarOpcao(int opcao) { // Método privado para executar a opção.
        switch (opcao) { // Usa uma estrutura 'switch-case' para direcionar a execução.
            case 0: // Se a opção for 0.
                abrirPasta(); // Chama o método para abrir pasta.
                break; // Sai do switch
            case 1:
                abrirArquivo();
                break;
            case 2:
                renomearArquivo();
                break;
            case 3:
                moverArquivo();
                break;
            case 4:
                copiarArquivo();
                break;
            case 5:
                excluirArquivo();
                break;
            case 6:
                criarArquivo();
                break;
            case 7:
                mostrarUsoDeMemoria(); // Chama o novo método para exibir o uso de memória.
                break;
            case 8:
                listarUnidadesDeDisco(); // Corrigido o nome da chamada do método
                break;
            case 9:
                System.out.println("Saindo do programa..."); // Corrigida a String
                break;
            default: // Se a opção não corresponder a nenhum dos casos anteriores.
                System.out.println("Opção inválida. Por favor, escolha uma opção válida."); // Mensagem de erro.
        }
    }

    private void abrirPasta() {
        System.out.print("Digite o caminho da pasta para abrir: ");
        String caminhoAbrir = scanner.nextLine(); // Lê o caminho da pasta.
        File pastaAbrir = new File(caminhoAbrir); // Cria um objeto File para a pasta.
        if (pastaAbrir.exists() && pastaAbrir.isDirectory()) { // Verifica se existe e é um diretório.
            System.out.println("Pasta encontrada: " + pastaAbrir.getAbsolutePath());
            salvarAcao("Pasta aberta: " + caminhoAbrir);
            // Aqui você pode adicionar código para listar o conteúdo da pasta, se necessário.
            // Por exemplo:
            // File[] arquivosNaPasta = pastaAbrir.listFiles();
            // if (arquivosNaPasta != null) {
            // System.out.println("\nConteúdo da pasta:");
            // for (File arquivo : arquivosNaPasta) {
            // System.out.println((arquivo.isDirectory() ? "[DIR] " : "[FILE] ") + arquivo.getName());
            // }
        } else {
            System.out.println("Pasta não encontrada ou o caminho não é um diretório.");
        }
    }

    /*
     * Solicita ao usuário o caminho de um arquivo e verifica se ele existe.
     */
    private void abrirArquivo() { // Método para abrir um arquivo.
        System.out.print("Digite o caminho do arquivo para abrir: "); // Solicita o caminho.
        String caminhoAbrir = scanner.nextLine(); // Lê o caminho.
        File arquivoAbrir = new File(caminhoAbrir);
        if (arquivoAbrir.exists() && arquivoAbrir.isFile()) { // Verifica se o arquivo existe e é um arquivo.
            System.out.println("Arquivo encontrado: " + arquivoAbrir.getAbsolutePath()); // Confirma que encontrou.
            salvarAcao("Arquivo aberto: " + caminhoAbrir); // Registra a ação no log.
            // abrir arquivo
            try (BufferedReader reader = new BufferedReader(new FileReader(arquivoAbrir))) {
                String linha;
                System.out.println("\nConteúdo do arquivo:");
                while ((linha = reader.readLine()) != null) {
                    System.out.println(linha);
                }
            } catch (IOException e) {
                System.out.println("Erro ao ler o arquivo: " + e.getMessage());
            }
        } else {
            System.out.println("Arquivo não encontrado."); // Informa que o arquivo não foi encontrado.
        }
    }

    /*
     * Solicita ao usuário o caminho original e o novo nome para um arquivo/diretório,
     * e então chama o método de renomeação.
     */
    private void renomearArquivo() { // Método para renomear um arquivo.
        System.out.print("Digite o caminho completo do arquivo/diretório a ser renomeado: ");
        String caminhoOriginalRenomear = scanner.nextLine();
        System.out.print("Digite o novo nome (apenas o nome do arquivo/diretório, sem caminho): ");
        String novoNome = scanner.nextLine();
        // Corrected parameter name from 'novonovoNome' to 'novoNome'
        renomearArquivo(caminhoOriginalRenomear, novoNome); // Chama o método de lógica principal.
        salvarAcao("Arquivo/diretório renomeado: " + caminhoOriginalRenomear + " para " + novoNome); // Registra a ação.
    }

    /*
     * Solicita ao usuário o caminho de origem e o caminho de destino para mover um arquivo/diretório,
     * e então chama o método de movimentação.
     */
    private void moverArquivo() { // Método para mover um arquivo.
        System.out.print("Digite o caminho completo do arquivo/diretório de origem para mover: ");
        String caminhoOrigemMover = scanner.nextLine();
        System.out.print("Digite o caminho completo do diretório de destino (incluindo o novo nome se desejar renomear): ");
        String caminhoDestinoMover = scanner.nextLine();
        moverArquivo(caminhoOrigemMover, caminhoDestinoMover); // Chama o método de lógica principal.
        salvarAcao("Arquivo/diretório movido: " + caminhoOrigemMover + " para " + caminhoDestinoMover); // Registra a ação.
    }

    /*
     * Solicita ao usuário o caminho de origem e o caminho de destino para copiar um arquivo,
     * e então chama o método de cópia.
     */
    private void copiarArquivo() { // Método para copiar um arquivo.
        System.out.print("Digite o caminho completo do arquivo de origem para copiar: ");
        String caminhoOrigemCopiar = scanner.nextLine();
        System.out.print("Digite o caminho completo do destino da cópia (incluindo o nome para a cópia): ");
        String caminhoDestinoCopiar = scanner.nextLine();
        copiarArquivo(caminhoOrigemCopiar, caminhoDestinoCopiar); // Chama o método de lógica principal.
        salvarAcao("Arquivo copiado: " + caminhoOrigemCopiar + " para " + caminhoDestinoCopiar); // Registra a ação.
    }

    /*
     * Solicita ao usuário o caminho de um arquivo/diretório a ser excluído,
     * e então chama o método de exclusão.
     */
    private void excluirArquivo() { // Método para excluir um arquivo.
        System.out.print("Digite o caminho completo do arquivo/diretório a ser excluído: ");
        String caminhoExcluir = scanner.nextLine();
        excluirArquivo(caminhoExcluir); // Chama o método de lógica principal.
        salvarAcao("Arquivo/diretório excluído: " + caminhoExcluir); // Registra a ação.
    }

    /*
     * Solicita ao usuário o caminho e o nome do arquivo a ser criado,
     * e então chama o método de criação.
     */
    private void criarArquivo() { // Método para criar um arquivo.
        System.out.print("Digite o caminho completo ou o nome do arquivo a ser criado: ");
        String caminhoCriar = scanner.nextLine();
        criarArquivo(caminhoCriar); // Chama o método de lógica principal.
        salvarAcao("Arquivo criado: " + caminhoCriar); // Registra a ação.
    }

    public void renomearArquivo(String caminho, String novoNome) { // Lógica para renomear.
        try { // Bloco try-catch para lidar com possíveis exceções.
            File arquivo = new File(caminho); // Cria um objeto File para o arquivo original.
            if (arquivo.exists()) { // Verifica se o arquivo/diretório original existe.
                File novoArquivo = new File(arquivo.getParent(), novoNome); // Cria um novo objeto File com o novo nome no mesmo diretório pai.
                if (arquivo.renameTo(novoArquivo)) { // Tenta renomear o arquivo.
                    System.out.println("Arquivo ou pasta renomeado com sucesso de '" + caminho + "' para '" + novoNome + "'");
                } else {
                    System.out.println("Erro ao renomear arquivo ou pasta. Verifique permissões ou se o novo nome já existe.");
                }
            } else {
                System.out.println("Arquivo ou pasta não encontrado: " + caminho); // Informa que o arquivo não foi encontrado.
            }
        } catch (Exception e) { // Captura qualquer exceção.
            System.out.println("Erro ao renomear arquivo ou pasta: " + e.getMessage()); // Imprime a mensagem de erro.
        }
    }

    /*
     * Move um arquivo ou diretório de um local para outro.
     */
    public void moverArquivo(String origem, String destino) { // Lógica para mover.
        try {
            File arquivoOrigem = new File(origem); // Objeto File para o arquivo de origem.
            File arquivoDestino = new File(destino); // Objeto File para o arquivo de destino.

            if (arquivoOrigem.exists()) { // Verifica se o arquivo de origem existe.
                // Garante que o diretório de destino exista.
                if (arquivoDestino.getParentFile() != null && !arquivoDestino.getParentFile().exists()) {
                    if (arquivoDestino.getParentFile().mkdirs()) { // Cria os diretórios pai se não existirem.
                        System.out.println("Diretórios de destino criados: " + arquivoDestino.getParentFile().getAbsolutePath());
                    } else {
                        System.out.println("Atenção: Não foi possível criar os diretórios de destino. Tentando mover assim mesmo.");
                    }
                }

                if (arquivoOrigem.renameTo(arquivoDestino)) { // Tenta mover o arquivo diretamente (mais eficiente).
                    System.out.println("Arquivo movido com sucesso de '" + origem + "' para '" + destino + "'");
                } else {
                    // Se renameTo falhar (ex: entre diferentes sistemas de arquivos), tenta copiar e depois deletar.
                    System.out.println("Erro ao mover o arquivo diretamente. Tentando copiar e deletar...");
                    copiarArquivo(origem, destino); // Tenta copiar o arquivo.
                    if (new File(destino).exists()) { // Se a cópia foi bem-sucedida.
                        if (arquivoOrigem.delete()) { // Tenta deletar o arquivo original.
                            System.out.println("Arquivo original excluído após cópia bem-sucedida.");
                        } else {
                            System.out.println("Erro ao excluir arquivo original após cópia. O arquivo pode existir em ambos os locais.");
                        }
                    }
                }
            } else {
                System.out.println("Arquivo de origem não encontrado: " + origem); // Informa que o arquivo de origem não foi encontrado.
            }
        } catch (Exception e) {
            System.out.println("Erro ao mover o arquivo: " + e.getMessage());
        }
    }

    /*
     * Copia um arquivo de um local para outro.
     * origem O caminho completo do arquivo de origem.
     * destino O caminho completo do destino da cópia (incluindo o nome para a cópia).
     */
    public void copiarArquivo(String origem, String destino) { // Lógica para copiar.
        try ( // 'try-with-resources' garante que os fluxos sejam fechados automaticamente.
            InputStream in = new FileInputStream(origem); // Abre um fluxo de entrada para o arquivo de origem.
            OutputStream out = new FileOutputStream(destino) // Abre um fluxo de saída para o arquivo de destino.
        ) {
            File arquivoOrigem = new File(origem); // Objeto File para o arquivo de origem.
            if (!arquivoOrigem.exists()) { // Verifica se o arquivo de origem existe.
                System.out.println("Arquivo de origem para cópia não encontrado: " + origem);
                return; // Sai do método se o arquivo não existir.
            }

            // Garante que o diretório de destino exista.
            File arquivoDestino = new File(destino); // Objeto File para o arquivo de destino.
            if (arquivoDestino.getParentFile() != null && !arquivoDestino.getParentFile().exists()) {
                if (arquivoDestino.getParentFile().mkdirs()) { // Cria os diretórios pai se não existirem.
                    System.out.println("Diretórios de destino criados para cópia: " + arquivoDestino.getParentFile().getAbsolutePath());
                } else {
                    System.out.println("Atenção: Não foi possível criar os diretórios de destino para cópia. Tentando copiar assim mesmo.");
                }
            }

            byte[] buffer = new byte[1024]; // Cria um buffer de 1KB para ler e escrever dados em blocos.
            int length; // Variável para armazenar o número de bytes lidos.
            // Lê do arquivo de origem e copia para o arquivo destinado.
            while ((length = in.read(buffer)) > 0) { // Enquanto houver bytes para ler.
                out.write(buffer, 0, length); // Escreve os bytes lidos no arquivo de destino.
            }
            System.out.println("Arquivo copiado com sucesso de '" + origem + "' para '" + destino + "'");
        } catch (IOException e) { // Captura exceções de I/O.
            System.out.println("Erro ao copiar o arquivo: " + e.getMessage()); // Imprime a mensagem de erro.
        }
    }

    /*
     * Exclui um arquivo ou diretório.
     *
     * caminho O caminho completo do arquivo/diretório a ser excluído.
     */
    public void excluirArquivo(String caminho) { // Lógica para excluir.
        File arquivo = new File(caminho); // Cria um objeto File para o arquivo/diretório a ser excluído.
        if (arquivo.exists()) { // Verifica se o arquivo/diretório existe.
            if (arquivo.delete()) { // Tenta excluir o arquivo/diretório.
                System.out.println("Arquivo ou diretório excluído com sucesso: " + caminho);
            } else {
                System.out.println("Erro ao excluir arquivo ou diretório. Verifique se não está em uso ou se possui permissões.");
            }
        } else {
            System.out.println("Arquivo ou diretório não encontrado para exclusão: " + caminho); // Informa que não foi encontrado.
        }
    }

    /*
     * Cria um novo arquivo vazio.
     * caminho O caminho completo e nome do arquivo a ser criado.
     */
    public void criarArquivo(String caminho) { // Lógica para criar arquivo.
        File arquivo = new File(caminho); // Cria um objeto File para o novo arquivo.
        try {
            // Garante que o diretório pai exista.
            if (arquivo.getParentFile() != null && !arquivo.getParentFile().exists()) {
                if (arquivo.getParentFile().mkdirs()) { // Cria os diretórios pai se não existirem.
                    System.out.println("Diretórios pai criados para o novo arquivo: " + arquivo.getParentFile().getAbsolutePath());
                } else {
                    System.out.println("Atenção: Não foi possível criar os diretórios pai. Tentando criar o arquivo assim mesmo.");
                }
            }

            if (arquivo.createNewFile()) { // Tenta criar o novo arquivo.
                System.out.println("Arquivo criado com sucesso: " + caminho);
            } else {
                System.out.println("Arquivo já existe ou erro ao criar: " + caminho);
            }
        } catch (IOException e) { // Captura exceções de I/O.
            System.out.println("Erro ao criar arquivo: " + e.getMessage()); // Imprime a mensagem de erro.
        }
    }

    /*
     * Lista as unidades de disco disponíveis no sistema.
     */
    private void listarUnidadesDeDisco() {
        File[] unidades = File.listRoots(); // Obtém um array de objetos File representando as unidades de disco.
        System.out.println("\n--- Unidades de Disco ---");
        if (unidades != null && unidades.length > 0) { // Verifica se há unidades e se o array não está vazio.
            for (File unidade : unidades) { // Itera sobre cada unidade de disco encontrada.
                System.out.println("Unidade: " + unidade.getAbsolutePath()); // Exibe o caminho absoluto da unidade (ex: C:\, D:\).
            }
            salvarAcao("Unidades de disco listadas."); // Registra a ação no log.
        } else {
            System.out.println("Nenhuma unidade de disco encontrada."); // Mensagem caso nenhuma unidade seja encontrada.
        }
    }

    /*
     * Exibe informações sobre o uso de memória da JVM.
     */
    private void mostrarUsoDeMemoria() { // Método para exibir o uso de memória.
        // Obtém o objeto Runtime associado à aplicação Java atual.
        Runtime runtime = Runtime.getRuntime();

        // Obtém a quantidade máxima de memória que a máquina virtual Java tentará usar.
        long memoriaMaxima = runtime.maxMemory();

        // Obtém a quantidade total de memória na máquina virtual Java.
        long memoriaTotal = runtime.totalMemory();

        // Obtém a quantidade de memória livre na máquina virtual Java.
        long memoriaLivre = runtime.freeMemory();

        // Calcula a memória utilizada (total alocada - livre).
        long memoriaUsada = memoriaTotal - memoriaLivre;

        System.out.println("----- Uso de Memória JVM -----"); // Changed to JVM for clarity
        System.out.println("Memória Máxima (bytes): " + memoriaMaxima);
        System.out.println("Memória Total (bytes): " + memoriaTotal);
        System.out.println("Memória Livre (bytes): " + memoriaLivre);
        System.out.println("Memória Usada (bytes): " + memoriaUsada);

        // Converte bytes para megabytes para facilitar a leitura.
        long memoriaMaximaMB = bytesParaMegabytes(memoriaMaxima);
        long memoriaTotalMB = bytesParaMegabytes(memoriaTotal);
        long memoriaLivreMB = bytesParaMegabytes(memoriaLivre);
        long memoriaUsadaMB = bytesParaMegabytes(memoriaUsada);

        System.out.println("\n----- Uso de Memória da JVM (MB) -----");
        System.out.println("Memória Máxima (MB): " + memoriaMaximaMB);
        System.out.println("Memória Total (MB): " + memoriaTotalMB);
        System.out.println("Memória Livre (MB): " + memoriaLivreMB);
        System.out.println("Memória Usada (MB): " + memoriaUsadaMB);
        salvarAcao("Uso de memória exibido."); // Registra a ação no log.
    }

    /*
     * Converte um valor de bytes para megabytes.
     */
    private static long bytesParaMegabytes(long bytes) { // Método auxiliar para conversão de unidades.
        return bytes / (1024 * 1024); // 1 MB = 1024 KB = 1024 * 1024 bytes.
    }

    /*
     * Salva uma ação no arquivo de log.
     * A string que descreve a ação a ser logada.
     */
    public void salvarAcao(String acao) { // Lógica para salvar ações no log.
        try (PrintWriter writer = new PrintWriter(new FileWriter(LOG_FILE, true))) { // Abre o arquivo de log em modo de anexação ('true').
            writer.println(acao); // Escreve a ação no arquivo, seguida por uma nova linha.
        } catch (IOException e) { // Captura exceções de I/O.
            System.out.println("Erro ao salvar ação no log: " + e.getMessage()); // Imprime a mensagem de erro.
        }
    }
}
