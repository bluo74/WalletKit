# WalletKit
WalletKit est un outil permettant de simplifier la gestion de l'argent dans le portefeuille

---

# 💼 WalletKit

**Documentation développeur — v1.0**
Outil pour NovaLife
Dévelloper par Bluo avec l'aide de JulesFR

---

## 📌 Présentation

**WalletKit** est un outil conçue pour simplifier la gestion du portefeuille (*wallet*) des joueurs.

Elle permet de :

* Lire le solde
* Ajouter de l'argent
* Retirer de l'argent

➡️ Sans manipuler directement l’inventaire ni la sérialisation JSON.

> ℹ️ L’ID de l’item wallet est défini dans `WalletItemId = 6060`.

---

## ⚙️ Installation

1. Ajouter `WalletKit.dll` dans le dossier de votre serveur :

```
plugins/
```

2. Ajoute la référence dans ton projet *pour les développers*

3. Importe le namespace :

```csharp
using WalletKit;
```

---

## 📦 Constantes

### `WalletItemId`

Identifiant de l’item wallet dans l’inventaire du joueur.

```csharp
public const int WalletItemId = 6060;
```

---

## 🧰 Méthodes

---

### 💰 GetCashInWallet

Retourne le total d'argent présent dans tous les wallets du joueur.

```csharp
public static double GetCashInWallet(Player player)
```

#### Paramètres

| Nom    | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| player | Player | Joueur dont on veut lire le solde |

#### Retour

* `double` → Montant total en euros
* Retourne `0` si aucun wallet trouvé

#### Exemple

```csharp
double solde = wallet.GetCashInWallet(player);
player.SendText($"Votre solde : {solde}€");
```

---

### ➕ AddCashToWallet

Ajoute de l'argent dans le premier wallet trouvé.

```csharp
public static bool AddCashToWallet(Player player, double amount)
```

#### Paramètres

| Nom    | Type   | Description                |
| ------ | ------ | -------------------------- |
| player | Player | Joueur qui reçoit l'argent |
| amount | double | Montant à ajouter (> 0)    |

#### Retour

* `true` → Succès
* `false` → Aucun wallet trouvé

#### Exemple

```csharp
bool succes = wallet.AddCashToWallet(player, 500.0);

if (!succes)
    player.SendText("Aucun wallet trouvé dans l'inventaire.");
```

---

### ➖ RemoveCashFromWallet

Retire de l'argent des wallets du joueur.

```csharp
public static bool RemoveCashFromWallet(Player player, double amount)
```

#### Paramètres

| Nom    | Type   | Description             |
| ------ | ------ | ----------------------- |
| player | Player | Joueur à débiter        |
| amount | double | Montant à retirer (> 0) |

#### Retour

* `true` → Montant retiré avec succès
* `false` → Fonds insuffisants ou aucun wallet

#### ⚠️ Important

* Vérifie le solde **avant** l'opération
* Si insuffisant → **aucun retrait effectué (atomique)**
* Supporte plusieurs wallets (vidés dans l'ordre)

#### Exemple

```csharp
bool succes = wallet.RemoveCashFromWallet(player, 200.0);

if (succes)
    player.SendText("Paiement de 200€ effectué.");
else
    player.SendText("Fonds insuffisants.");
```

---

## ✅ Bonnes pratiques

* ✔️ Toujours vérifier le `bool` retourné
* ❌ Ne jamais passer un montant ≤ 0
* ✔️ Vérifier le solde avec `GetCashInWallet()` avant un paiement

---

## 🛒 Exemple complet : système d'achat

```csharp
public void AcheterVoiture(Player player, double prix)
{
    double solde = wallet.GetCashInWallet(player);

    if (solde < prix)
    {
        player.SendText($"Fonds insuffisants. Vous avez {solde}€, besoin de {prix}€.");
        return;
    }

    bool retire = wallet.RemoveCashFromWallet(player, prix);

    if (retire)
        player.SendText($"Achat réussi ! {prix}€ débité.");
    else
        player.SendText("Erreur lors du paiement.");
}
```

---

## 📖 Résumé

WalletKit te permet de gérer facilement :

* 💰 Lecture du solde
* ➕ Ajout d'argent
* ➖ Retrait sécurisé

Sans te soucier de la logique interne du wallet.

---

## 🧩 À propos

> WalletKit — *Aide pour la gestion du porte-monnaie*
Discord BlueHub - https://discord.gg/cdfHH7pn2x

## 👨‍💻 Crédits

- Outil développé par **Bluo**
- Logique principale réalisée avec l'aide de JulesFR
