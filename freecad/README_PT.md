# Engrenagens Encaixadas no FreeCAD

Este guia explica como fazer duas engrenagens onde a menor se encaixa dentro da maior e ambas giram juntas.

## Conceito: Engrenagem Interna (Coroa) + Pinhão

Quando você quer que uma engrenagem menor gire **dentro** de uma engrenagem maior, você precisa de:

1. **Engrenagem Interna (Coroa/Ring Gear)**: Uma engrenagem com dentes voltados para **dentro**
2. **Pinhão (Pinion)**: Uma engrenagem externa menor que encaixa dentro da coroa

![Diagrama de Engrenagem Interna](https://upload.wikimedia.org/wikipedia/commons/8/8f/Internal_gear.svg)

## Método 1: Usando o Script Python (Automático)

### Passos:

1. Abra o FreeCAD
2. Vá em **View → Panels → Python console**
3. Execute o script:
   ```python
   exec(open("/caminho/para/interlocking_gears.py").read())
   ```

4. As engrenagens serão criadas automaticamente com os parâmetros padrão

### Personalizando os Parâmetros:

Edite o arquivo `interlocking_gears.py` e modifique:

```python
# Módulo (deve ser igual para ambas engrenagens!)
MODULE = 2.0  # mm

# Número de dentes da engrenagem interna (coroa)
INTERNAL_GEAR_TEETH = 40

# Número de dentes do pinhão
PINION_TEETH = 15

# Ângulo de pressão (padrão: 20°)
PRESSURE_ANGLE = 20.0  # graus

# Espessura das engrenagens
GEAR_THICKNESS = 10.0  # mm
```

## Método 2: Usando o FCGear Workbench (Recomendado)

O FCGear cria engrenagens com perfil involuto preciso, ideal para fabricação.

### Instalação do FCGear:

1. Vá em **Tools → Addon Manager**
2. Pesquise por "FCGear"
3. Instale e reinicie o FreeCAD

### Criando a Engrenagem Interna:

1. Mude para o workbench **FCGear**
2. Clique em **Internal Involute Gear** (Engrenagem Involuta Interna)
3. Configure:
   - **Teeth**: Número de dentes (ex: 40)
   - **Module**: Módulo em mm (ex: 2.0)
   - **Pressure Angle**: Ângulo de pressão (ex: 20°)
   - **Height**: Altura/espessura (ex: 10mm)

### Criando o Pinhão:

1. Clique em **Involute Gear** (Engrenagem Involuta)
2. Configure com o **MESMO módulo e ângulo de pressão**:
   - **Teeth**: Número de dentes menor (ex: 15)
   - **Module**: 2.0 (deve ser igual!)
   - **Pressure Angle**: 20° (deve ser igual!)
   - **Height**: 10mm

### Posicionando o Pinhão:

A distância entre centros é calculada por:

```
Distância = Módulo × (Dentes_Coroa - Dentes_Pinhão) / 2
```

Exemplo: `2.0 × (40 - 15) / 2 = 25 mm`

Para posicionar:
1. Selecione o pinhão
2. Vá em **View → Panels → Placement** (ou pressione P)
3. Mude **Position X** para o valor calculado (ex: 25 mm)

## Fazendo as Engrenagens Girarem Juntas

### Usando Spreadsheet (Planilha):

1. Crie uma nova planilha: **Insert → Spreadsheet**
2. Na célula A1, digite o ângulo de rotação do pinhão (ex: 0)
3. Na célula B1, digite a fórmula: `=A1 * 15/40` (substitua pelos seus dentes)
4. Vincule a rotação do pinhão à célula A1
5. Vincule a rotação da coroa à célula B1
6. Mude A1 para animar ambas as engrenagens!

### Relação de Transmissão:

Para engrenagens internas, a relação é:

```
Rotação_Coroa = Rotação_Pinhão × (Dentes_Pinhão / Dentes_Coroa)
```

**IMPORTANTE**: Diferente de engrenagens externas, em engrenagens internas ambas giram na **MESMA direção**!

## Regras para Engrenamento Correto

Para que as engrenagens encaixem e girem corretamente:

1. ✅ **Mesmo Módulo**: Ambas devem ter o mesmo módulo
2. ✅ **Mesmo Ângulo de Pressão**: Geralmente 20°
3. ✅ **Dentes Diferentes**: A coroa deve ter mais dentes que o pinhão
4. ✅ **Distância Correta**: Use a fórmula de distância entre centros
5. ✅ **Diferença Mínima de Dentes**: A coroa deve ter pelo menos 10-15 dentes a mais

## Solução de Problemas

### Engrenagens não encaixam:
- Verifique se o módulo é igual
- Recalcule a distância entre centros

### Dentes colidem ou há folga excessiva:
- Ajuste a distância entre centros
- Verifique o ângulo de pressão

### Engrenagem interna parece invertida:
- É normal! Os dentes devem apontar para dentro
- Se usar FCGear, use "Internal Involute Gear", não "Involute Gear"

## Exportando para Fabricação

1. Selecione a engrenagem
2. Vá em **File → Export**
3. Escolha o formato:
   - **.STEP** - Para usinagem CNC
   - **.STL** - Para impressão 3D
   - **.DXF** - Para corte laser (exporte a face)

## Referências

- [Documentação do FCGear](https://wiki.freecadweb.org/FCGear_Workbench)
- [Teoria de Engrenagens](https://en.wikipedia.org/wiki/Gear)
- [Engrenagens Internas](https://en.wikipedia.org/wiki/Gear#Internal_gear)
