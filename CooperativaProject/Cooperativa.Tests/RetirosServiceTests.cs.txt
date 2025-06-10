using NUnit.Framework;
using Cooperativa;

namespace Cooperativa.Tests
{
    [TestFixture]
    public class CuentaAhorrosServiceTests
    {
        private CuentaAhorrosService _service;

        [SetUp]
        public void Setup()
        {
            _service = new CuentaAhorrosService();
        }

        [Test]
        [Description("TC1: Retiro exitoso con todas las condiciones válidas (Happy Path).")]
        public void EsRetiroValido_CuandoTodasLasCondicionesSonValidas_DebeRetornarTrue()
        {
            // Arrange: Definir los datos de entrada directamente.
            var estado = "activa";
            var saldo = 500m;
            var bloqueada = false;
            var monto = 100m;
            var limite = 200m;

            // Act: Ejecutar el método a probar con los parámetros individuales.
            var resultado = _service.EsRetiroValido(estado, saldo, bloqueada, monto, limite);

            // Assert: Verificar que el resultado es el esperado (verdadero).
            Assert.IsTrue(resultado, "El retiro debería ser aprobado si todas las condiciones son válidas.");
        }

        // Casos de prueba para las condiciones inválidas (TC2 a TC6)
        [TestCase("inactiva", 500, false, 100, 200, TestName = "TC2: Falla si la cuenta está inactiva")]
        [TestCase("activa", 50, false, 100, 200, TestName = "TC3: Falla si el saldo es insuficiente")]
        [TestCase("activa", 500, false, 250, 200, TestName = "TC4: Falla si excede el límite de retiro diario")]
        [TestCase("activa", 500, true, 100, 200, TestName = "TC5: Falla si la cuenta está bloqueada por fraude")]
        [TestCase("activa", 500, false, 105, 200, TestName = "TC6: Falla si el monto no es múltiplo de 10")]
        [TestCase("activa", 500, false, 0, 200, TestName = "TC7: Falla si el monto es cero")]
        [TestCase("activa", 500, false, -100, 200, TestName = "TC8: Falla si el monto es negativo")]
        public void EsRetiroValido_CuandoUnaCondicionEsInvalida_DebeRetornarFalse(string estado, decimal saldo, bool bloqueada, decimal monto, decimal limite)
        {
            // Act: Llamar al servicio directamente con los datos del TestCase.
            var resultado = _service.EsRetiroValido(estado, saldo, bloqueada, monto, limite);

            // Assert
            Assert.IsFalse(resultado);
        }
    }
}