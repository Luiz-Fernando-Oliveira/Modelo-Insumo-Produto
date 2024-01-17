# Modelo-Insumo-Produto
Análise de Impacto com o Modelo de Insumo-Produto no Python

# Código 
# bibliotecas

import pandas as pd

import numpy as np

# Código

## Lendo a base de dados

caminho = "C:/Users/lfro9/OneDrive/Documentos/Contas Sociais e Análise Insumo-Produto/"

arquivo = "MIP_estimada_em_aula.xlsx"

local = caminho + arquivo

MIP = pd.read_excel(local)

## Definindo as variáveis de interesse

Z = MIP.iloc[1:69,1:69]

DF = MIP.iloc[1:69,69:75]

X = MIP.iloc[155,1:69]

# Verificando o tipo das variáveis e ajustando

print(type(Z))

Z = Z.astype(float)

DF = DF.astype(float)

X = X.astype(float)


# Calculando o modelo canônico de Leontief

Y = np.sum(DF, axis=1)

Xchapeu = np.diag(X)

print(type(Xchapeu))

Xchapeu = Xchapeu.astype(float)

Xchapeu_inversa = np.linalg.inv(Xchapeu)

A = Z @ Xchapeu_inversa

A = A.astype(float)

I = np.eye(68)

L = np.linalg.inv(I-A)

X_estimado = L @ Y

# Checando a estimativa

vErro = X - X_estimado

nErro = np.sum(vErro, axis=0)

# Análise de impacto, supondo o crescimento de 1 (milhões de reais) em cada setor da demanda final

deltaY = np.full((68,1), 1)

deltaX = L @ deltaY

ndeltaX = np.sum(deltaX, axis = 0)

print("o impacto no VBP do aumento de 1 milhão de reais em cada setor foi de", ndeltaX, "milhões de reais")

linhaVA = 141

PrimeiroSetor = 1

UltimoSetor = 69

VA = MIP.iloc[linhaVA, PrimeiroSetor:UltimoSetor]

VA = VA.astype(float)

vRelacaoVA = VA @ Xchapeu_inversa

nVariacaoVA = vRelacaoVA @ deltaX

print("o impacto no VA do aumento de 1 milhão de reais em cada setor foi de", nVariacaoVA, "milhões de reais")

linhaEmprego = 154

Emprego = MIP.iloc[linhaEmprego, PrimeiroSetor:UltimoSetor]

Emprego = Emprego.astype(float)

vRelacaoEmprego = Emprego @ (Xchapeu_inversa*1000000)

nVariacaoEmprego = vRelacaoEmprego @ deltaX

print("o impacto no Emprego do aumento de 1 milhão de reais em cada setor foi de", nVariacaoEmprego)

# Calculando Backward e Foward linkages

# Resultados
<class 'pandas.core.frame.DataFrame'>
<class 'numpy.ndarray'>
o impacto no VBP do aumento de 1 milhão de reais em cada setor foi de [123.5743718] milhões de reais
o impacto no VA do aumento de 1 milhão de reais em cada setor foi de [53.28304256] milhões de reais
o impacto no Emprego do aumento de 1 milhão de reais em cada setor foi de [9.73511289e+08]
