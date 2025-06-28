# SimpleUniswapRouter

## Descripción

`SimpleUniswapRouter` es un contrato inteligente que implementa un intercambio descentralizado (DEX) simple inspirado en Uniswap V2. Permite a los usuarios añadir y remover liquidez entre pares de tokens ERC20, así como realizar swaps de tokens a través de rutas personalizadas.

Incluye un token de liquidez interno (`LiquidityToken`) para representar la participación de cada usuario en el pool.

---

## Contratos principales

- **LiquidityToken:** Token ERC20 personalizado que representa la participación de un usuario en un pool de liquidez. Permite mint y burn de tokens cuando se añaden o retiran liquidez.

- **SimpleUniswapRouter:** Contrato principal que gestiona los pools, las reservas, las operaciones de añadir/remover liquidez y el intercambio de tokens.

---

## Funcionalidades principales

### Añadir liquidez

```solidity
function addLiquidity(
    address tokenA,
    address tokenB,
    uint256 amountADesired,
    uint256 amountBDesired,
    uint256 amountAMin,
    uint256 amountBMin,
    address to,
    uint256 deadline
) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);

    Añade liquidez a un par de tokens.

    Calcula automáticamente las cantidades óptimas según las reservas existentes.

    Minta tokens de liquidez proporcionales al aporte.

    Requiere aprobación previa para transferir los tokens del usuario al contrato.

Remover liquidez

function removeLiquidity(
    address tokenA,
    address tokenB,
    uint256 liquidity,
    uint256 amountAMin,
    uint256 amountBMin,
    address to,
    uint256 deadline
) external returns (uint256 amountA, uint256 amountB);

    Permite retirar tokens del pool devolviendo tokens de liquidez.

    Calcula las cantidades según la proporción de liquidez retirada.

    Transfiere los tokens retirados a la dirección especificada.

Realizar swap exacto de tokens

function swapExactTokensForTokens(
    uint256 amountIn,
    uint256 amountOutMin,
    address[] calldata path,
    address to,
    uint256 deadline
) external returns (uint256[] memory amounts);

    Intercambia una cantidad exacta de tokens amountIn siguiendo una ruta definida (path).

    Requiere que la salida mínima amountOutMin se cumpla o la transacción revierte.

    Actualiza reservas y transfiere los tokens correspondientes.

Consultas auxiliares

    getReserves(tokenA, tokenB): Devuelve las reservas actuales de ambos tokens en el pool.

    getAmountOut(amountIn, reserveIn, reserveOut): Calcula la cantidad de salida para un swap dado el input y las reservas.

    getPrice(tokenA, tokenB): Retorna el precio actual (tokenB/tokenA) en el pool, con 18 decimales de precisión.

Token de Liquidez (LiquidityToken)

    ERC20 estándar con funciones adicionales:

        mint(address to, uint256 amount): Permite crear nuevos tokens.

        burn(address from, uint256 amount): Permite quemar tokens existentes.

Notas importantes

    Todas las transferencias de tokens requieren que el usuario haya aprobado previamente al contrato para gastar sus tokens (approve).

    Los deadlines evitan la ejecución fuera de tiempo para proteger contra front-running y otros ataques.

    El fee aplicado a swaps es del 0.3% (997/1000).

Uso básico

    Agregar liquidez:

        Aprueba el contrato para gastar tus tokens A y B.

        Llama a addLiquidity con las cantidades deseadas.

    Realizar swap:

        Aprueba el contrato para gastar el token de entrada.

        Llama a swapExactTokensForTokens con la ruta deseada y cantidades.

    Remover liquidez:

        Llama a removeLiquidity con la cantidad de tokens de liquidez a quemar.

Requisitos para compilar y desplegar

    Solidity ^0.8.20

    Compatible con tokens ERC20 estándar.

Autor

Sergio Gallo
