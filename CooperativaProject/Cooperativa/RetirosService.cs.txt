// Archivo: CuentaAhorrosService.cs
using System;

namespace Cooperativa
{
    public class CuentaAhorrosService
    {
        /// <summary>
        /// Valida si un retiro puede ser realizado en una cuenta de ahorros.
        /// </summary>
        /// <param name="estadoCuenta">El estado actual de la cuenta (ej. "activa").</param>
        /// <param name="saldoCuenta">El saldo disponible en la cuenta.</param>
        /// <param name="cuentaBloqueada">Indica si la cuenta está bloqueada por fraude.</param>
        /// <param name="montoARetirar">El monto que el cliente desea retirar.</param>
        /// <param name="limiteDiario">El límite de retiro diario para este tipo de cuenta.</param>
        /// <returns>True si el retiro es válido, de lo contrario False.</returns>
        public bool EsRetiroValido(string estadoCuenta, decimal saldoCuenta, bool cuentaBloqueada, decimal montoARetirar, decimal limiteDiario)
        {
            // 1. Condición: La cuenta debe estar activa.
            if (estadoCuenta != "activa") return false;

            // 2. Condición: El cliente debe tener saldo suficiente.
            if (saldoCuenta < montoARetirar) return false;

            // 3. Condición: El monto no debe exceder el límite de retiro diario.
            if (montoARetirar > limiteDiario) return false;

            // 4. Condición: La cuenta no debe estar bloqueada por fraude.
            if (cuentaBloqueada) return false;

            // 5. Condición: El monto debe ser múltiplo de 10.
            if (montoARetirar % 10 != 0) return false;

            // 6. Condición: El monto debe ser mayor que 0.
            if (montoARetirar <= 0) return false;

            // Si todas las condiciones se cumplen, el retiro es válido.
            return true;
        }
    }
}