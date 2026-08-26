# 💼 Contract Processing & Income Calculation Engine

[![Java 25](https://img.shields.io/badge/Java-25-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://jdk.java.net/25/)
[![POO](https://img.shields.io/badge/Architecture-Object%20Composition-00599C?style=for-the-badge)](https://en.wikipedia.org/wiki/Object_composition)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

> **Módulo de Gestão de Contratos e Composição de Renda**: Aplicação Java desenvolvida para processamento autônomo de contratos de prestação de serviços por departamento, calculando dinamicamente a receita total consolidada de um colaborador para qualquer período específico (Mês/Ano).

---

## 💡 Destaques de Arquitetura e Engenharia

Este projeto foca na aplicação prática e limpa dos pilares do paradigma de **Orientação a Objetos**:

- **Composição de Objetos (Object Composition):**
  - **Relacionamento `1:1`:** Um `Worker` pertence a um `Department`.
  - **Relacionamento `1:N`:** Um `Worker` possui múltiplos contratos de hora de trabalho (`HourContract`), manipulados de forma segura através de uma lista encapsulation interna.
- **Tipagem Forte e Enums (`WorkerLevel`):** Uso de enumerações para categorização funcional (`JUNIOR`, `MID_LEVEL`, `SENIOR`), evitando *magic numbers* ou strings soltas no domínio.
- **Manipulação Moderna de Datas (`java.time`):** Processamento e parsing de datas utilizando a API nativa `LocalDate` e `DateTimeFormatter`, garantindo precisão ao filtrar contratos por mês e ano (`getYear()`, `getMonthValue()`).
- **Encapsulamento e Ocultação de Informação:** A lista de contratos do colaborador não é exposta diretamente via setters; as alterações são feitas exclusivamente por métodos de negócio expressivos (`addContract` e `removeContract`).

---

## ⚙️ Regras de Domínio e Negócio

### 1. Cálculo da Renda Total (`income(year, month)`)
A receita final calculada para um trabalhador em um dado período é a soma do seu **Salário Base** com o **Valor Total dos Contratos** vigentes naquele mês/ano específico:

$$\text{Renda Total} = \text{Salário Base} + \sum (\text{Valor por Hora} \times \text{Horas Truncadas})_{\text{Mês/Ano}}$$

### 2. Associação do Contrato
Cada contrato individual (`HourContract`) é composto por:
- **Data do Contrato:** `LocalDate`
- **Valor por Hora:** `Double`
- **Duração (Horas):** `Integer`

---

## 📐 Diagrama de Classes (UML)

```mermaid
classDiagram
    class Department {
        - String name
        + getName() String
        + setName(String name)
    }

    class WorkerLevel {
        <<enumeration>>
        JUNIOR
        MID_LEVEL
        SENIOR
    }

    class HourContract {
        - LocalDate date
        - Double valuePerHour
        - Integer hours
        + totalValue() Double
        + getDate() LocalDate
    }

    class Worker {
        - String name
        - WorkerLevel level
        - Double baseSalary
        - Department department
        - List~HourContract~ contracts
        + addContract(HourContract contract)
        + removeContract(HourContract contract)
        + income(int year, int month) Double
    }

    Worker "1" --> "1" Department : "belongs to"
    Worker "1" --> "1" WorkerLevel : "has"
    Worker "1" *-- "*" HourContract : "has many"
