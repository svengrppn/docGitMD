#Les sessions en PHP 

--- 

1. Créer une page index.php avec un formulaire dedans :

```html title="index.php" linenums="1" 
<!DOCTYPE html>
<html>
<head>
    <title>Connexion</title>
</head>
<body>

<h1>Connexion</h1>    

``` 
```php linenums="9"
<?php if (isset($error)): ?>
    <p><?php echo $error; ?></p>
<?php endif; ?>
```

```html linenums="12"
<form method="post" action="">
    <div>
        <label for="username">Nom d'utilisateur:</label>
        <input type="text" id="username" name="username">
    </div>
    <div>
        <label for="password">Mot de passe:</label>
        <input type="password" id="password" name="password">
    </div>
    <div>
        <button type="submit">Se connecter</button>
    </div>
</form>

</body>
</html>

```
2. Démarrer la session avec la function session_start() : 

```php linenums="1"

<?php
session_start();
?>

```
3. Créer un fichier json qui contiendra les noms et le mot de passe  ( haché ) des utilisateurs  : 

```json title="users.json" linenums="1"

{
    "user": {
        "password": "$2y$10$78eV23pkAYWTv.pfl9dp9eI/Nu0nSQlJTN8thGzH6eQmRoBOFI1iC"
    },
    "admin": {
        "password": "$2y$10$BDb5Qxc2MebjZg2/YuHqh.NPCsh5zjc4O/qnzcQ3zKnAW5vUdpNDm"
    }
}


```
4. Récupérer les valeurs du formulaire avec des variables php et on valide si les identifiants sont corrects :

```php title="index.php" linenums="1"

    <?php
    session_start();

    // Vérifier si le formulaire a été soumis
    if (isset($_POST['username']) && isset($_POST['password'])) {
    // Charger les identifiants depuis le fichier JSON
    $users = json_decode(file_get_contents('users.json'), true);

    // Récupérer les informations du formulaire
    $username = $_POST['username'];
    $password = $_POST['password'];

    // Vérifier si l'utilisateur existe dans le fichier JSON
    if (isset($users[$username])) {
        // Vérifier si le mot de passe est correct
        if (password_verify($password, $users[$username]['password'])) {
            // Connexion réussie, enregistrer le nom d'utilisateur en session
            $_SESSION['username'] = $username;
            header('Location: validation.php');
            exit;
        }else{
            $error = "Mot de passe incorrect.";
        }
    }else{
        $error = "Nom d'utilisateur incorrect."; 
    }

}

?>

```





