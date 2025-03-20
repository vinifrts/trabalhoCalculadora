public class Lexico {
    private static final String OPERADORES = "~^v<>";
    private static final String VARIAVEIS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private static final String PARENTESES = "()";

    public static String verificarFormula(String formula) {
        for (char c : formula.toCharArray()) {

            if (Character.isWhitespace(c)) {
                continue;
            }

            // ve se é valido a letra
            if (OPERADORES.indexOf(c) == -1 && VARIAVEIS.indexOf(c) == -1 && PARENTESES.indexOf(c) == -1) {
                return "não";
            }
        }
        return "sim";
    }

    public static void main(String[] args) {
        String formula1 = "(A<B)^(C>D)"; // Fórmula 1
        String formula2 = "~(AvB)"; // Fórmula 2

        System.out.println(" a 1 é válida? " + verificarFormula(formula1));
        System.out.println(" a 2 é válida? " + verificarFormula(formula2));
    }
}

