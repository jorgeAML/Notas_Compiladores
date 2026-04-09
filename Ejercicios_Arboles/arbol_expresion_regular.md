# Método del Árbol — Expresión Regular: (usac)*(compi1)+(B)

## 1. Análisis de la expresión

```
(usac)*(compi1)+(B)
```

Se descompone en la concatenación (·) de tres partes:
- (usac)*    → "usac" con cerradura de Kleene
- (compi1)+  → "compi1" con cerradura positiva
- (B)        → literal B

Nota: (compi1)+ equivale a (compi1)·(compi1)*

## 2. Expansión completa

```
(u·s·a·c)* · (c·o·m·p·i·1) · (c·o·m·p·i·1)* · B
```

## 3. Árbol Sintáctico

```
                                    ·(14)
                                  /       \
                               ·(13)       B(15)
                             /       \
                          ·(12)      *(11)
                        /      \       |
                     ·(7)    ·(10)   ·(10')
                    /    \   /    \   /    \
                 *(6)  ·(9) ...  1  ...   1
                  |   / \
                ·(5) c  ·(8)
               / \     / \
             ·(4) c   o  ·(8')
            / \         / \
          ·(3) a       m  ·(8'')
         / \             / \
        u   s           p   i
```

Dado que el árbol completo es muy extenso, lo represento de forma
más clara por niveles:

```
                                          · ←── raíz (concatenación final)
                                        /   \
                                      ·       B
                                    /   \
                                  ·       *
                                /   \     |
                              *     ·     · (c·o·m·p·i·1)
                              |   / | \
                              ·  c  o  ·
                            / | \    / | \
                           u  s  ·  m  p  ·
                                |       / \
                                ·      i   1
                               / \
                              a   c
```

## 4. Árbol detallado con numeración de hojas (posiciones)

Las hojas se numeran de izquierda a derecha (1, 2, 3, ...):

```
                                              ·
                                           /     \
                                         ·        B₁₅
                                      /      \
                                    ·          *
                                 /     \        \
                               *        ·        ·
                               |      / | \    / | \
                               ·    c₅ o₆ ·  c₁₁o₁₂·
                             / | \      / | \     / | \
                           u₁ s₂ ·    m₇ p₈ ·  m₁₃p₁₄·
                                 |         / \       / \
                                 ·       i₉  1₁₀  i₁₃ 1₁₄
                                / \
                              a₃  c₄
```

## 5. Cálculo de Anulable, Primeros y Últimos

### Hojas:
| Pos | Símbolo | Anulable | Primeros | Últimos |
|-----|---------|----------|----------|---------|
| 1   | u       | No       | {1}      | {1}     |
| 2   | s       | No       | {2}      | {2}     |
| 3   | a       | No       | {3}      | {3}     |
| 4   | c       | No       | {4}      | {4}     |
| 5   | c       | No       | {5}      | {5}     |
| 6   | o       | No       | {6}      | {6}     |
| 7   | m       | No       | {7}      | {7}     |
| 8   | p       | No       | {8}      | {8}     |
| 9   | i       | No       | {9}      | {9}     |
| 10  | 1       | No       | {10}     | {10}    |
| 11  | c       | No       | {11}     | {11}    |
| 12  | o       | No       | {12}     | {12}    |
| 13  | m       | No       | {13}     | {13}    |
| 14  | p       | No       | {14}     | {14}    |
| 15  | B       | No       | {15}     | {15}    |

### Nodos internos (de abajo hacia arriba):

**Concatenaciones de (usac):**
- u·s:       Anulable=No, Primeros={1}, Últimos={2}
- (u·s)·a:   Anulable=No, Primeros={1}, Últimos={3}
- (u·s·a)·c: Anulable=No, Primeros={1}, Últimos={4}

**(usac)*:**
- Anulable=**Sí**, Primeros={1}, Últimos={4}

**Concatenaciones de (compi1) — primera ocurrencia:**
- c·o:         Anulable=No, Primeros={5}, Últimos={6}
- (c·o)·m:     Anulable=No, Primeros={5}, Últimos={7}
- (c·o·m)·p:   Anulable=No, Primeros={5}, Últimos={8}
- (c·o·m·p)·i: Anulable=No, Primeros={5}, Últimos={9}
- (c·o·m·p·i)·1: Anulable=No, Primeros={5}, Últimos={10}

**(usac)* · (compi1):**
- Anulable=No, Primeros={1,5}, Últimos={10}
  (Primeros incluye {5} porque (usac)* es anulable)

**Concatenaciones de (compi1) — segunda ocurrencia (para el *):**
- Similar: Primeros={11}, Últimos={14} (ajustando posiciones)

**(compi1)*:**
- Anulable=**Sí**, Primeros={11}, Últimos={14}

**(usac)*(compi1) · (compi1)*:**
- Anulable=No, Primeros={1,5}, Últimos={10,14}
  (Últimos incluye {14} porque (compi1)* podría tener elementos)

**· B (concatenación final):**
- Anulable=No, Primeros={1,5}, Últimos={15}

## 6. Tabla de Siguientes

| Pos | Símbolo | Siguientes          |
|-----|---------|---------------------|
| 1   | u       | {2}                 |
| 2   | s       | {3}                 |
| 3   | a       | {4}                 |
| 4   | c       | {1,5}               |
| 5   | c       | {6}                 |
| 6   | o       | {7}                 |
| 7   | m       | {8}                 |
| 8   | p       | {9}                 |
| 9   | i       | {10}                |
| 10  | 1       | {11,15}             |
| 11  | c       | {12}                |
| 12  | o       | {13}                |
| 13  | m       | {14}                |
| 14  | p       | {11,15}             |
| 15  | B       | ∅ (aceptación)      |

### Justificación de siguientes clave:
- Pos 4 (c de usac): por el *, puede volver a {1} o avanzar a {5}
- Pos 10 (1 de compi1): avanza a {11} (inicio del *) o a {15} (B)
- Pos 14 (p de compi1*): por el *, puede volver a {11} o avanzar a {15}

## 7. Construcción del AFD

**Estado inicial:** Primeros de la raíz = {1, 5}

| Estado | Posiciones  | u     | s     | a     | c       | o     | m     | p       | i     | 1       | B     |
|--------|-------------|-------|-------|-------|---------|-------|-------|---------|-------|---------|-------|
| A      | {1,5}       | {2}   | -     | -     | {6}     | -     | -     | -       | -     | -       | -     |
| B      | {2}         | -     | {3}   | -     | -       | -     | -     | -       | -     | -       | -     |
| C      | {3}         | -     | -     | {4}   | -       | -     | -     | -       | -     | -       | -     |
| D      | {4}         | -     | -     | -     | {1,5}   | -     | -     | -       | -     | -       | -     |
| E      | {6}         | -     | -     | -     | -       | -     | {7}   | -       | -     | -       | -     |
| F→E    | {6}=E       |       |       |       |         |       |       |         |       |         |       |
| G      | {7}         | -     | -     | -     | -       | -     | -     | {8}     | -     | -       | -     |
| H      | {8}         | -     | -     | -     | -       | -     | -     | -       | {9}   | -       | -     |
| I      | {9}         | -     | -     | -     | -       | -     | -     | -       | -     | {10}    | -     |
| J      | {10}        | -     | -     | -     | {12}    | -     | -     | -       | -     | -       | {15}* |
| K      | {11}        | -     | -     | -     | {12}    | -     | -     | -       | -     | -       | -     |
| L      | {11,15}     | -     | -     | -     | {12}    | -     | -     | -       | -     | -       | **✓** |
| M      | {12}        | -     | -     | -     | -       | -     | {13}  | -       | -     | -       | -     |
| N      | {13}        | -     | -     | -     | -       | -     | -     | {14}    | -     | -       | -     |
| O      | {14}        | -     | -     | -     | {11,15} | -     | -     | -       | -     | -       | -     |

**Estado de aceptación:** cualquiera que contenga la posición 15.
