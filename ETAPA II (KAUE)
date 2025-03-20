import java.util.Stack;

public class Sintatico {
    // Símbolos válidos
    private static final String OPERADORES = "~^v><";
    private static final String VARIAVEIS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private static final String PARENTESES = "()";

    // Classe para retornar o resultado da análise
    private static class ResultadoAnalise {
        boolean sucesso;
        int posicaoFinal;
        String mensagemErro;

        ResultadoAnalise(boolean sucesso, int posicaoFinal) {
            this.sucesso = sucesso;
            this.posicaoFinal = posicaoFinal;
            this.mensagemErro = "";
        }

        ResultadoAnalise(boolean sucesso, int posicaoFinal, String mensagemErro) {
            this.sucesso = sucesso;
            this.posicaoFinal = posicaoFinal;
            this.mensagemErro = mensagemErro;
        }
    }

    // Verifica se uma fórmula é uma FBF (Fórmula Bem Formada)
    public static String verificarFBF(String formula) {
        formula = formula.replaceAll("\\s+", ""); // Remove espaços

        if (formula.isEmpty()) {
            return "Não, porque a fórmula está vazia.";
        }

        // Verifica caracteres inválidos
        String caracterInvalido = verificarCaracteresValidos(formula);
        if (!caracterInvalido.isEmpty()) {
            return "Não, porque a fórmula contém o caractere inválido: '" + caracterInvalido + "'";
        }

        // Verifica parênteses balanceados
        String erroParenteses = verificarParentesesBalanceados(formula);
        if (!erroParenteses.isEmpty()) {
            return "Não, porque " + erroParenteses;
        }

        // Analisa a expressão
        ResultadoAnalise resultado = analisarExpressao(formula, 0);
        if (!resultado.sucesso) {
            return "Não, porque " + (resultado.mensagemErro.isEmpty() ?
                    "a estrutura da fórmula não segue as regras de uma FBF" : resultado.mensagemErro);
        }

        // Verifica se toda a fórmula foi analisada
        if (resultado.posicaoFinal < formula.length()) {
            return "Não, porque há símbolos extras após uma expressão válida na posição " + (resultado.posicaoFinal + 1);
        }

        return "Sim, a fórmula é uma FBF.";
    }

    // Verifica se os parênteses estão balanceados
    private static String verificarParentesesBalanceados(String formula) {
        Stack<Integer> pilha = new Stack<>();
        for (int i = 0; i < formula.length(); i++) {
            char c = formula.charAt(i);
            if (c == '(') {
                pilha.push(i);
            } else if (c == ')') {
                if (pilha.isEmpty()) {
                    return "há um parêntese de fechamento sem abertura correspondente na posição " + (i + 1);
                }
                pilha.pop();
            }
        }

        if (!pilha.isEmpty()) {
            return "há um parêntese de abertura sem fechamento correspondente na posição " + (pilha.peek() + 1);
        }

        return ""; // Parênteses balanceados
    }

    // Verifica se todos os caracteres são válidos
    private static String verificarCaracteresValidos(String formula) {
        for (int i = 0; i < formula.length(); i++) {
            char c = formula.charAt(i);
            if (VARIAVEIS.indexOf(c) == -1 && OPERADORES.indexOf(c) == -1 && PARENTESES.indexOf(c) == -1) {
                return String.valueOf(c);
            }
        }
        return "";
    }

    // Analisa expressão recursivamente
    private static ResultadoAnalise analisarExpressao(String formula, int posicao) {
        if (posicao >= formula.length()) {
            return new ResultadoAnalise(false, posicao, "a expressão termina abruptamente");
        }

        // Primeiro analisamos um termo (variável, negação ou expressão entre parênteses)
        ResultadoAnalise termo = analisarTermo(formula, posicao);
        if (!termo.sucesso) {
            return termo;
        }

        // Se ainda houver caracteres e o próximo for um operador binário,
        // continuamos a análise
        if (termo.posicaoFinal < formula.length() &&
                OPERADORES.indexOf(formula.charAt(termo.posicaoFinal)) != -1 &&
                formula.charAt(termo.posicaoFinal) != '~') {

            char operador = formula.charAt(termo.posicaoFinal);

            // Analisamos o próximo termo após o operador
            ResultadoAnalise proximoTermo = analisarExpressao(formula, termo.posicaoFinal + 1);
            if (!proximoTermo.sucesso) {
                return new ResultadoAnalise(false, termo.posicaoFinal,
                        "após o operador '" + operador + "': " + proximoTermo.mensagemErro);
            }

            return proximoTermo;
        }

        // Se não há mais operadores, retorna o sucesso do termo atual
        return termo;
    }

    // Analisa um termo (variável, negação ou expressão entre parênteses)
    private static ResultadoAnalise analisarTermo(String formula, int posicao) {
        if (posicao >= formula.length()) {
            return new ResultadoAnalise(false, posicao, "a expressão termina abruptamente");
        }

        char c = formula.charAt(posicao);

        // Caso 1: Variável
        if (VARIAVEIS.indexOf(c) != -1) {
            return new ResultadoAnalise(true, posicao + 1);
        }

        // Caso 2: Negação
        if (c == '~') {
            if (posicao + 1 >= formula.length()) {
                return new ResultadoAnalise(false, posicao, "o operador de negação (~) não é seguido por uma expressão");
            }

            ResultadoAnalise resultado = analisarTermo(formula, posicao + 1);
            if (!resultado.sucesso) {
                return new ResultadoAnalise(false, posicao, "após o operador de negação (~): " + resultado.mensagemErro);
            }

            return resultado;
        }

        // Caso 3: Expressão entre parênteses
        if (c == '(') {
            ResultadoAnalise expressao = analisarExpressao(formula, posicao + 1);
            if (!expressao.sucesso) {
                return new ResultadoAnalise(false, posicao, "dentro dos parênteses: " + expressao.mensagemErro);
            }

            if (expressao.posicaoFinal >= formula.length() || formula.charAt(expressao.posicaoFinal) != ')') {
                return new ResultadoAnalise(false, posicao, "falta um parêntese de fechamento após a expressão");
            }

            return new ResultadoAnalise(true, expressao.posicaoFinal + 1);
        }

        // Não é um termo válido
        return new ResultadoAnalise(false, posicao, "'" + c + "' não inicia um termo válido");
    }

    public static void main(String[] args) {
        // Exemplos de teste
        String[] formulas = {
                "(A>B)^(B>A)",
                "A^C",
                "(A>B))^(B>A)",
                "A)) ^^ > BC",
                "~A",
                "(~A>B)",
                "A^B^C",
                "(A^(B>C))",
                "(A&B)",
                "r"
        };

        for (int i = 0; i < formulas.length; i++) {
            System.out.println("Fórmula " + (i + 1) + ": " + formulas[i]);
            System.out.println(verificarFBF(formulas[i]));
            System.out.println();
        }
    }
}
