# StellarWorkshopAbidjan (Côte D'Ivoire 🇨🇮)

![Stellar](https://img.shields.io/badge/Stellar-Network-blue) ![Soroban](https://img.shields.io/badge/Soroban-Smart%20Contracts-green) ![Rust](https://img.shields.io/badge/Rust-Programming-orange)

Toutes les directives de base et avancées pour démarrer votre **Parcours de Développeur Stellar** !
- (par la communauté Stellar Côte D'Ivoire 🇨🇮)

## Table des Matières
- [Étape 1 : Comprendre les Bases](#étape-1--comprendre-les-bases)
- [Étape 2 : Configurer Votre Environnement de Développement](#étape-2--configurer-votre-environnement-de-développement)
- [Étape 3 : Écrire Votre Premier Contrat Intelligent](#étape-3--écrire-votre-premier-contrat-intelligent)
- [Étape 4 : Compiler le Contrat](#étape-4--compiler-le-contrat)
- [Étape 5 : Déployer le Contrat](#étape-5--déployer-le-contrat)
- [Étape 6 : Interagir avec le Contrat](#étape-6--interagir-avec-le-contrat)
- [Étape 7 : Tester Votre Contrat](#étape-7--tester-votre-contrat)

---

## Étape 1 : Comprendre les Bases

Avant de plonger dans le développement, familiarisez-vous avec ces concepts fondamentaux :

- **Contrats Intelligents** - Contrats auto-exécutables avec des termes directement écrits dans le code
- **Réseau Stellar** - Une plateforme blockchain décentralisée pour les paiements transfrontaliers
- **Soroban** - La plateforme de contrats intelligents de Stellar, conçue pour l'évolutivité et l'expérience développeur

### Ressources
- [Documentation Développeur Stellar](https://developers.stellar.org/)
- [Documentation Soroban](https://soroban.stellar.org/)

---

## Étape 2 : Configurer Votre Environnement de Développement

### Prérequis
Assurez-vous d'avoir installé les éléments suivants sur votre système :

#### Installer Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```

#### Installer Soroban CLI
```bash
cargo install --locked soroban-cli
```

#### Configurer un Réseau de Développement Local
```bash
soroban network add local http://localhost:8000/soroban/rpc --local
soroban config identity generate alice
```

> **Note :** Ceci crée un environnement de test local pour vos contrats intelligents.

---

## Étape 3 : Écrire Votre Premier Contrat Intelligent

### Créer un Nouveau Projet
```bash
soroban contract init hello-world
cd hello-world
```

### Ajouter les Dépendances
Modifiez votre fichier `Cargo.toml` :
```toml
[dependencies]
soroban-sdk = "20.0.0"
```

### Écrire le Contrat
Créez la logique de votre contrat intelligent dans `src/lib.rs` :
```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, log, Env, Symbol, symbol_short};

#[contract]
pub struct IncrementContract;

#[contractimpl]
impl IncrementContract {
    /// Incrémenter un compteur interne et retourner la valeur
    pub fn increment(env: Env, user: Symbol, value: u32) -> u32 {
        // Implémentation ici
        log!(&env, "Incrémentation du compteur pour l'utilisateur : {}", user);
        value + 1
    }
}
```

---

## Étape 4 : Compiler le Contrat

Compilez votre contrat intelligent :
```bash
soroban contract build
```

Ceci génère un fichier `.wasm` dans le répertoire `target/wasm32-unknown-unknown/release/`.

---

## Étape 5 : Déployer le Contrat

Déployez votre contrat sur le réseau local :
```bash
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/hello_world.wasm \
  --source alice \
  --network local
```

Sauvegardez l'ID de contrat retourné pour les interactions futures.

---

## Étape 6 : Interagir avec le Contrat

### Initialiser le Contrat
```bash
soroban contract invoke \
  --id CONTRACT_ID \
  --source alice \
  --network local \
  -- initialize
```

### Incrémenter
```bash
soroban contract invoke \
  --id CONTRACT_ID \
  --source alice \
  --network local \
  -- increment --user alice --value 5
```

### Obtenir la Valeur Actuelle
```bash
soroban contract invoke \
  --id CONTRACT_ID \
  --source alice \
  --network local \
  -- get --user alice
```

---

## Étape 7 : Tester Votre Contrat

### Écrire des Tests
Créez des tests complets dans `src/test.rs` :
```rust
#[cfg(test)]
mod test {
    use super::*;
    use soroban_sdk::Env;

    #[test]
    fn test_increment() {
        let env = Env::default();
        let contract_id = env.register_contract(None, IncrementContract);
        let client = IncrementContractClient::new(&env, &contract_id);

        let result = client.increment(&symbol_short!("alice"), &1);
        assert_eq!(result, 2);
    }
}
```

### Exécuter les Tests
```bash
cargo test
```

Pour des tests plus complets :
```bash
soroban contract test
```

---

## 🎉 Félicitations !

Vous avez terminé avec succès votre premier contrat intelligent Stellar ! 

### Que Faire Ensuite ?
- Explorer des modèles de contrats plus complexes
- Apprendre sur le système d'actifs de Stellar
- Construire des applications décentralisées (dApps)
- Rejoindre la communauté de développeurs Stellar

### Ressources Utiles
- [Stellar Quest](https://quest.stellar.org/) - Plateforme d'apprentissage interactive
- [Discord Communauté Stellar](https://discord.gg/stellar-dev)
- [Exemples Soroban](https://github.com/stellar/soroban-examples)

---

## Contribution

Les contributions sont les bienvenues ! N'hésitez pas à soumettre une Pull Request.

## Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

*Fait avec ❤️ par la Communauté des Builders Stellar d'Abidjan (Côte D'Ivoire 🇨🇮)*
