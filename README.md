# SimpleSwap - Mini DEX con Tokens ERC20 personalizados

Este proyecto contiene:

- Dos tokens ERC20 personalizados: **TokenA** y **TokenB**, con suministro inicial de 10,000 unidades cada uno.
- Un token de liquidez **LiquidityToken** para representar participación en pools.
- Un contrato de intercambio **SimpleSwap**, que permite agregar/remover liquidez y hacer swaps simples con protección contra slippage.

---

## Tabla de contenidos

- [Contratos](#contratos)  
- [Funciones principales](#funciones-principales)  
- [Variables públicas](#variables-públicas)  
- [Eventos](#eventos)  
- [Cómo usar](#cómo-usar)  
- [Pasos para desplegar](#pasos-para-desplegar)

---

## Contratos

### LiquidityToken

Token ERC20 minimalista para representar tokens de liquidez emitidos por SimpleSwap.

### SimpleSwap

Contrato que gestiona pools de liquidez para pares de tokens ERC20, permite:

- Agregar liquidez
- Remover liquidez
- Intercambiar tokens (swaps) con control de slippage

---

## Funciones principales

### LiquidityToken

- `mint(address to, uint256 amount)`: Emite tokens de liquidez a una dirección.
- `burn(address from, uint256 amount)`: Quema tokens de liquidez de una dirección.
- `transfer`, `approve`, `transferFrom`: Funciones estándar ERC20.

### SimpleSwap

- `_pairKey(address tokenA, address tokenB) internal pure returns (bytes32)`: Calcula una clave única para el par de tokens.
- `getReserves(address tokenA, address tokenB) public view returns (uint reserveA, uint reserveB)`: Retorna las reservas actuales del par.
- `addLiquidity(...) external returns (uint amountA, uint amountB, uint liquidity)`: Agrega liquidez al pool, calculando cantidades óptimas.
- `removeLiquidity(...) external returns (uint amountA, uint amountB)`: Remueve liquidez proporcional al token de liquidez quemado.
- `swapExactTokensForTokens(...) external returns (uint amountOut)`: Realiza un swap de tokens con protección contra slippage.
- `getPrice(address tokenA, address tokenB) external view returns (uint price)`: Retorna el precio del tokenA en términos del tokenB.
- `getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) public pure returns (uint amountOut)`: Calcula la cantidad de token de salida esperada según las reservas y el monto de entrada.

---

## Variables públicas

- `LiquidityToken public liquidityToken`: Referencia al token de liquidez.
- `mapping(bytes32 => Reserves) public reserves`: Mapa que almacena las reservas por par de tokens.

---

## Eventos

- `Transfer(address indexed from, address indexed to, uint256 value)` (ERC20 estándar)
- `Approval(address indexed owner, address indexed spender, uint256 value)` (ERC20 estándar)

---

## Cómo usar

1. Desplegar los tokens **TokenA** y **TokenB** con suministro inicial.
2. Desplegar el contrato **SimpleSwap**.
3. Aprobar a SimpleSwap para que transfiera tokens TokenA y TokenB (usando `approve` de cada token).
4. Agregar liquidez con `addLiquidity` enviando las cantidades deseadas.
5. Usar `swapExactTokensForTokens` para intercambiar tokens entre TokenA y TokenB.
6. Remover liquidez con `removeLiquidity` para recuperar tokens y quemar tokens de liquidez.

---

## Pasos para desplegar y probar en Remix

1. Abrir Remix IDE y crear los archivos `TokenA.sol`, `TokenB.sol` y `SimpleSwap.sol` con el código correspondiente.
2. Compilar con Solidity versión 0.8.20 o compatible.
3. Desplegar los contratos TokenA y TokenB primero.
4. Copiar las direcciones desplegadas de TokenA y TokenB.
5. Desplegar SimpleSwap (sin argumentos).
6. Desde Remix, ejecutar `approve` en TokenA y TokenB para permitir que SimpleSwap transfiera tokens (usualmente `approve(SimpleSwapAddress, amount)`).
7. Ejecutar `addLiquidity` en SimpleSwap con los tokens y cantidades.
8. Hacer swaps y remover liquidez según necesidad.
