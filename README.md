# rust-compiler-experiments

## Can I use Traits to generate inlined, monomorphized code?

Looks like yes. Only the function implementation will need an inline attribute:
```rust
pub trait Get {
    fn get();
}

pub struct Traitor;
impl Get for Traitor {
    #[inline(always)]
    fn get() {
        unsafe {
            asm!("cli" :::: "volatile");
        }
    }
}
```
- [Link to Godbolt](https://rust.godbolt.org/#z:OYLghAFBqd5QCxAYwPYBMCmBRdBLAF1QCcAaPECAKxAEZSAbAQwDtRiBXAZwNK9Q7FkmEAHIApACYAzGDDiArACEAZpiYFBmCEy4BbAJSKAIuIAMAQXMWADhwBGAah6dkBRwDkNeAG6Zx0krW1nh6Ngye3n6O4gDsQZaOSTEyikp4LAwZ2kwMAO5MAJ5cRgqmicl2TiosjhlZLJgA%2BrkFxU3AmAQQBjHx1smDjhwsXExqfQkWQzOOunpyEFKSLKg2y44gWyApkj6ozAR4DP6SkkaBA7OD84vLq%2Btnm9u7%2B4fHp%2BcBU9dx5dPJP7BCpJKTSNL1bImK5JKqOGp1TLZDpdHqTGFDEZjCZxH7XZK3SDLZBZDbbHbLN7eE7LC54/FzfR3M4kvBkl6Ug7Uz50jGDIEgvr/DFwhGdbq9XF85JY8aYdGC66EpZnPDELrsravLlHGlnXmK2bK5ZqjVPcna956r6XQ2g2L/fkO4LOyzWOEEYhMQiOADiXQVANBqWUkMaOnyRRK0MFCLDzVaUZREsD11lOP6dqGxrOD01FLOVN1PO%2B0pmOZWa3zlu5tNLWYFQaFwKbYIhSMaMabcY7zXFaKlWfT8sHTaVTKJLNJ5o5hZ1HzrtrHRonKskrOrnKtJaXv1drf3GLFqMlmeXSWHqYZjIWk8kpoIm7n28X9PxFYfT728%2BtBvPzcFRsj1qeMWkjdpxSaHgHB6esrEPSw4RcDg3EcAAVL1CBIODQnCP0AxUEh0MwohiCvYNwVDXsIzaaMymAxEGgTcCuGTKDNHsAczzTUY5XI99V2JadJGeLUt1rfU4OvG9mXXYTRILb8X0k3dZkbJ1hQQ2wHHhWo9G9FguLfLwjj8LZQMTCCTyk5ITN8EQQFA/s/0GOyzJAZy4IxDDvVI8zezA2i2OgziXOSHysOIfymMCpNPNUpIIr8xyAvit8kpILY0pdYxRAMRgxAUURSBYMQzGK1AxAAJW4dx%2BEEYQUmkWhioIMq8vygBrEAFDMArRAAFmK0rRHK0hKtEYquBAPq2tGvLSDgWAkDQMIPjICgIFWmx1pAFg8GABACAYQpSBUY4CEwYhpogex2tIewMiYYhCjEFrSFWvRMBYAgAHlMle%2BbSCwfS2BOe78HVNx7OmoHMAAD0wZAOEut7ioyS6GHuz1Qna/LmDYEBOB4Rg8HsabIHytYjlQUYxCmgQhBEWh8cK4b7om4mCGQRx9sO47CkcCBcEipr6EcABhVA1pOMiwRZxwap4Vq8a6nq%2Bqxwb2aBiappm0g5vKgxWdESRtbG3WDdV0g/GuvBaZAAagA)
