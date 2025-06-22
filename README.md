# 🦄 SimpleSwap

**SimpleSwap** es un contrato inteligente de intercambio descentralizado (DEX) inspirado en Uniswap, que permite agregar y remover liquidez, intercambiar tokens ERC-20 y consultar precios y cantidades resultantes.

## ✨ Características principales

- Permite agregar y remover liquidez a pares de tokens.
- Calcula precios automáticamente según reservas.
- Admite swaps entre dos tokens ERC-20.
- Implementa un token ERC-20 personalizado para representar liquidez (`LiquidityToken`).
- Seguridad básica con protección contra reentradas.
- Eventos de seguimiento (`AddLiquidity`, `RemoveLiquidity`, `Swap`).
- Validaciones estrictas (`require`).

---

## 🧱 Contratos

### `LiquidityToken.sol`

Implementa un token ERC20 minimalista con funciones de:

- `mint()` para emitir tokens de liquidez (solo usado internamente).
- `burn()` para destruir tokens al remover liquidez.
- `transfer`, `approve`, `transferFrom` con validaciones y protección `nonReentrant`.

### `SimpleSwap.sol`

Contrato principal que maneja:

- Pares de tokens (con almacenamiento interno de reservas).
- Agregado y retiro de liquidez (`addLiquidity`, `removeLiquidity`).
- Swaps con validación de slippage (`swapExactTokensForTokens`).
- Consulta de precios (`getPrice`) y simulación de swaps (`getAmountOut`).

---

## 📘 Funciones principales

### 🔹 `addLiquidity(...)`

Agrega liquidez a un par de tokens. Emite `AddLiquidity`.

```solidity
function addLiquidity(
  address tokenA,
  address tokenB,
  uint amountADesired,
  uint amountBDesired,
  uint amountAMin,
  uint amountBMin,
  address to,
  uint deadline
) external returns (uint amountA, uint amountB, uint liquidity);

🔹 removeLiquidity(...)

Retira liquidez y devuelve tokens al usuario. Emite RemoveLiquidity.

function removeLiquidity(
  address tokenA,
  address tokenB,
  uint liquidity,
  uint amountAMin,
  uint amountBMin,
  address to,
  uint deadline
) external returns (uint amountA, uint amountB);

🔹 swapExactTokensForTokens(...)

Intercambia tokens de entrada por salida exacta. Emite Swap.

function swapExactTokensForTokens(
  uint amountIn,
  uint amountOutMin,
  address[] calldata path,
  address to,
  uint deadline
) external returns (uint amountOut);

🔹 getPrice(tokenA, tokenB)

Devuelve el precio actual de tokenA en términos de tokenB.

function getPrice(address tokenA, address tokenB) external view returns (uint);

🔹 getAmountOut(amountIn, reserveIn, reserveOut)

Calcula la cantidad de salida que se recibiría por amountIn.
🔐 Seguridad

    Protección contra reentradas (nonReentrant).

    Validaciones de timestamp (deadline) y slippage (amountOutMin).

    Validación de tokens nulos o montos inválidos.

🧪 Ejemplo de uso

    Aprobar los tokens a intercambiar:

IERC20(tokenA).approve(address(simpleSwap), amount);
IERC20(tokenB).approve(address(simpleSwap), amount);

    Agregar liquidez:

simpleSwap.addLiquidity(tokenA, tokenB, 1000, 1000, 900, 900, msg.sender, block.timestamp + 300);

    Intercambiar tokens:

address ;
path[0] = tokenA;
path[1] = tokenB;

simpleSwap.swapExactTokensForTokens(500, 450, path, msg.sender, block.timestamp + 300);

📁 Estructura del proyecto

📦 SimpleSwap
 ┣ 📜 SimpleSwap.sol
 ┣ 📜 LiquidityToken.sol
 ┗ 📜 README.md
