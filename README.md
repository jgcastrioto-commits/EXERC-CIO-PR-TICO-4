<!DOCTYPE html>
<html lang="pt-br">
<head>
   <meta charset="UTF-8">
   <title>Página Inicial</title>
</head>
<body>
<h2>Login simples</h2>
<form method="POST" action="login.php">
   Usuário: <input type="text" name="usuario"><br>
   Senha: <input type="password" name="senha"><br>
   <select name="tema">
       <option value="claro">Claro</option>
       <option value="escuro">Escuro</option>
   </select><br>
   <input type="submit" value="Entrar">
  </form>
</body>
</html>

<?php
session_start();
$usuario = $_POST['usuario'] ?? '';
$senha = $_POST['senha'] ?? '';
if ($usuario === 'admin' && $senha === '123') {
   $_SESSION['usuario'] = $usuario;
   echo "Login bem-sucedido! <a href='produtos.php'>Ir para produtos</a>";
} else {
   echo "Usuário ou senha incorretos. <a href='index.php'>Tentar novamente</a>";
}
$_SESSION['tema'] = $_POST['tema'];
?>

<?php
session_start();
if (!isset($_SESSION['usuario'])) {
   echo "Acesso negado. <a href='index.php'>Faça login</a>";
   exit;
}
$tema = $_SESSION['tema'] ?? 'claro';
if ($tema === 'escuro') {
    echo "<body style='background-color: #021bfaff; color: #faf9f9ff'>";
} else {
    echo "<body style='background-color: #fc2010ff; color: #000'>";
}

$produtos = ['camisa', 'calça', 'sapato', 'boné', 'meia'];
$filtro = $_GET['busca'] ?? '';

echo "<h2>Olá, " . $_SESSION['usuario'] . "!</h2>";

echo "<form method='GET'>
   Buscar produto: <input type='text' name='busca' value='$filtro'>
   <input type='submit' value='Pesquisar'>
</form>";

echo "<h3>Lista de produtos:</h3>";
echo "<ul>";
foreach ($produtos as $p) {
    if ($filtro === '' || str_contains(strtolower($p), strtolower($filtro))) {
        echo "<li>$p</li>";
    }
}
echo "</ul>";

echo "<h3>Tabela de produtos:</h3>";
echo "<table border='1'>";
echo "<tr><th>Nome do Produto</th></tr>";
foreach ($produtos as $p) {
    if ($filtro === '' || str_contains(strtolower($p), strtolower($filtro))) {
        echo "<tr><td>$p</td></tr>";
    }
}
echo "</table>";

echo "<br><a href='index.php'>Sair</a>";
?>
