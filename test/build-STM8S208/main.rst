                                      1 ;--------------------------------------------------------
                                      2 ; File Created by SDCC : free open source ISO C Compiler 
                                      3 ; Version 4.4.0 #14620 (Linux)
                                      4 ;--------------------------------------------------------
                                      5 	.module main
                                      6 	.optsdcc -mstm8
                                      7 	
                                      8 ;--------------------------------------------------------
                                      9 ; Public variables in this module
                                     10 ;--------------------------------------------------------
                                     11 	.globl _main
                                     12 	.globl _uart1_puts
                                     13 	.globl _init_uart1
                                     14 	.globl _milis
                                     15 	.globl _init_milis
                                     16 	.globl _GPIO_ReadInputPin
                                     17 	.globl _GPIO_ReadOutputData
                                     18 	.globl _GPIO_WriteReverse
                                     19 	.globl _GPIO_WriteLow
                                     20 	.globl _GPIO_WriteHigh
                                     21 	.globl _GPIO_Write
                                     22 	.globl _GPIO_Init
                                     23 	.globl _CLK_HSIPrescalerConfig
                                     24 	.globl _sprintf
                                     25 ;--------------------------------------------------------
                                     26 ; ram data
                                     27 ;--------------------------------------------------------
                                     28 	.area DATA
                                     29 ;--------------------------------------------------------
                                     30 ; ram data
                                     31 ;--------------------------------------------------------
                                     32 	.area INITIALIZED
                                     33 ;--------------------------------------------------------
                                     34 ; Stack segment in internal ram
                                     35 ;--------------------------------------------------------
                                     36 	.area SSEG
      009332                         37 __start__stack:
      009332                         38 	.ds	1
                                     39 
                                     40 ;--------------------------------------------------------
                                     41 ; absolute external ram data
                                     42 ;--------------------------------------------------------
                                     43 	.area DABS (ABS)
                                     44 
                                     45 ; default segment ordering for linker
                                     46 	.area HOME
                                     47 	.area GSINIT
                                     48 	.area GSFINAL
                                     49 	.area CONST
                                     50 	.area INITIALIZER
                                     51 	.area CODE
                                     52 
                                     53 ;--------------------------------------------------------
                                     54 ; interrupt vector
                                     55 ;--------------------------------------------------------
                                     56 	.area HOME
      008000                         57 __interrupt_vect:
      008000 82 00 80 6F             58 	int s_GSINIT ; reset
      008004 82 00 86 10             59 	int _TRAP_IRQHandler ; trap
      008008 82 00 86 11             60 	int _TLI_IRQHandler ; int0
      00800C 82 00 86 12             61 	int _AWU_IRQHandler ; int1
      008010 82 00 86 13             62 	int _CLK_IRQHandler ; int2
      008014 82 00 86 14             63 	int _EXTI_PORTA_IRQHandler ; int3
      008018 82 00 86 15             64 	int _EXTI_PORTB_IRQHandler ; int4
      00801C 82 00 86 16             65 	int _EXTI_PORTC_IRQHandler ; int5
      008020 82 00 86 17             66 	int _EXTI_PORTD_IRQHandler ; int6
      008024 82 00 86 18             67 	int _EXTI_PORTE_IRQHandler ; int7
      008028 82 00 86 19             68 	int _CAN_RX_IRQHandler ; int8
      00802C 82 00 86 1A             69 	int _CAN_TX_IRQHandler ; int9
      008030 82 00 86 1B             70 	int _SPI_IRQHandler ; int10
      008034 82 00 86 1C             71 	int _TIM1_UPD_OVF_TRG_BRK_IRQHandler ; int11
      008038 82 00 86 1D             72 	int _TIM1_CAP_COM_IRQHandler ; int12
      00803C 82 00 86 1E             73 	int _TIM2_UPD_OVF_BRK_IRQHandler ; int13
      008040 82 00 86 1F             74 	int _TIM2_CAP_COM_IRQHandler ; int14
      008044 82 00 86 20             75 	int _TIM3_UPD_OVF_BRK_IRQHandler ; int15
      008048 82 00 86 21             76 	int _TIM3_CAP_COM_IRQHandler ; int16
      00804C 82 00 86 22             77 	int _UART1_TX_IRQHandler ; int17
      008050 82 00 86 23             78 	int _UART1_RX_IRQHandler ; int18
      008054 82 00 86 24             79 	int _I2C_IRQHandler ; int19
      008058 82 00 86 25             80 	int _UART3_TX_IRQHandler ; int20
      00805C 82 00 86 26             81 	int _UART3_RX_IRQHandler ; int21
      008060 82 00 86 27             82 	int _ADC2_IRQHandler ; int22
      008064 82 00 86 28             83 	int _TIM4_UPD_OVF_IRQHandler ; int23
      008068 82 00 86 42             84 	int _EEPROM_EEC_IRQHandler ; int24
                                     85 ;--------------------------------------------------------
                                     86 ; global & static initialisations
                                     87 ;--------------------------------------------------------
                                     88 	.area HOME
                                     89 	.area GSINIT
                                     90 	.area GSFINAL
                                     91 	.area GSINIT
      00806F CD 89 54         [ 4]   92 	call	___sdcc_external_startup
      008072 4D               [ 1]   93 	tnz	a
      008073 27 03            [ 1]   94 	jreq	__sdcc_init_data
      008075 CC 80 6C         [ 2]   95 	jp	__sdcc_program_startup
      008078                         96 __sdcc_init_data:
                                     97 ; stm8_genXINIT() start
      008078 AE 00 00         [ 2]   98 	ldw x, #l_DATA
      00807B 27 07            [ 1]   99 	jreq	00002$
      00807D                        100 00001$:
      00807D 72 4F 00 00      [ 1]  101 	clr (s_DATA - 1, x)
      008081 5A               [ 2]  102 	decw x
      008082 26 F9            [ 1]  103 	jrne	00001$
      008084                        104 00002$:
      008084 AE 00 04         [ 2]  105 	ldw	x, #l_INITIALIZER
      008087 27 09            [ 1]  106 	jreq	00004$
      008089                        107 00003$:
      008089 D6 80 CF         [ 1]  108 	ld	a, (s_INITIALIZER - 1, x)
      00808C D7 00 00         [ 1]  109 	ld	(s_INITIALIZED - 1, x), a
      00808F 5A               [ 2]  110 	decw	x
      008090 26 F7            [ 1]  111 	jrne	00003$
      008092                        112 00004$:
                                    113 ; stm8_genXINIT() end
                                    114 	.area GSFINAL
      008092 CC 80 6C         [ 2]  115 	jp	__sdcc_program_startup
                                    116 ;--------------------------------------------------------
                                    117 ; Home
                                    118 ;--------------------------------------------------------
                                    119 	.area HOME
                                    120 	.area HOME
      00806C                        121 __sdcc_program_startup:
      00806C CC 82 9A         [ 2]  122 	jp	_main
                                    123 ;	return from main will return to caller
                                    124 ;--------------------------------------------------------
                                    125 ; code
                                    126 ;--------------------------------------------------------
                                    127 	.area CODE
                                    128 ;	src/main.c: 32: static void seg7_show(uint8_t digit, bool blank)
                                    129 ; genLabel
                                    130 ;	-----------------------------------------
                                    131 ;	 function seg7_show
                                    132 ;	-----------------------------------------
                                    133 ;	Register assignment might be sub-optimal.
                                    134 ;	Stack space usage: 2 bytes.
      00819B                        135 _seg7_show:
      00819B 89               [ 2]  136 	pushw	x
                                    137 ; genReceive
                                    138 ;	src/main.c: 34: uint8_t p    = blank ? 0 : SEG7[digit % 10];
                                    139 ; genIfx
      00819C 0D 05            [ 1]  140 	tnz	(0x05, sp)
      00819E 26 03            [ 1]  141 	jrne	00177$
      0081A0 CC 81 A7         [ 2]  142 	jp	00119$
      0081A3                        143 00177$:
                                    144 ; genAssign
      0081A3 4F               [ 1]  145 	clr	a
                                    146 ; genGoto
      0081A4 CC 81 B4         [ 2]  147 	jp	00120$
                                    148 ; genLabel
      0081A7                        149 00119$:
                                    150 ; skipping iCode since result will be rematerialized
                                    151 ; genCast
                                    152 ; genAssign
      0081A7 5F               [ 1]  153 	clrw	x
                                    154 ; genIPush
      0081A8 4B 0A            [ 1]  155 	push	#0x0a
      0081AA 4B 00            [ 1]  156 	push	#0x00
                                    157 ; genSend
      0081AC 97               [ 1]  158 	ld	xl, a
                                    159 ; genCall
      0081AD CD 8A 8D         [ 4]  160 	call	__modsint
                                    161 ; genPlus
      0081B0 1C 80 95         [ 2]  162 	addw	x, #(_SEG7+0)
                                    163 ; genPointerGet
      0081B3 F6               [ 1]  164 	ld	a, (x)
                                    165 ; genCast
                                    166 ; genAssign
                                    167 ; genCast
                                    168 ; genAssign
                                    169 ; genLabel
      0081B4                        170 00120$:
                                    171 ; genCast
                                    172 ; genAssign
                                    173 ; genCast
                                    174 ; genAssign
      0081B4 6B 01            [ 1]  175 	ld	(0x01, sp), a
                                    176 ;	src/main.c: 36: uint8_t val  = 0;
                                    177 ; genAssign
      0081B6 5F               [ 1]  178 	clrw	x
                                    179 ;	src/main.c: 38: if (p & 0x01) val |= SEG_A;
                                    180 ; genAnd
      0081B7 7B 01            [ 1]  181 	ld	a, (0x01, sp)
      0081B9 44               [ 1]  182 	srl	a
      0081BA 25 03            [ 1]  183 	jrc	00178$
      0081BC CC 81 C2         [ 2]  184 	jp	00102$
      0081BF                        185 00178$:
                                    186 ; skipping generated iCode
                                    187 ; genAssign
      0081BF A6 08            [ 1]  188 	ld	a, #0x08
      0081C1 95               [ 1]  189 	ld	xh, a
                                    190 ; genLabel
      0081C2                        191 00102$:
                                    192 ;	src/main.c: 39: if (p & 0x02) val |= SEG_B;
                                    193 ; genAnd
      0081C2 7B 01            [ 1]  194 	ld	a, (0x01, sp)
      0081C4 A5 02            [ 1]  195 	bcp	a, #0x02
      0081C6 26 03            [ 1]  196 	jrne	00179$
      0081C8 CC 81 CF         [ 2]  197 	jp	00104$
      0081CB                        198 00179$:
                                    199 ; skipping generated iCode
                                    200 ; genOr
      0081CB 9E               [ 1]  201 	ld	a, xh
      0081CC AA 10            [ 1]  202 	or	a, #0x10
      0081CE 95               [ 1]  203 	ld	xh, a
                                    204 ; genLabel
      0081CF                        205 00104$:
                                    206 ;	src/main.c: 40: if (p & 0x04) val |= SEG_C;
                                    207 ; genAnd
      0081CF 7B 01            [ 1]  208 	ld	a, (0x01, sp)
      0081D1 A5 04            [ 1]  209 	bcp	a, #0x04
      0081D3 26 03            [ 1]  210 	jrne	00180$
      0081D5 CC 81 DC         [ 2]  211 	jp	00106$
      0081D8                        212 00180$:
                                    213 ; skipping generated iCode
                                    214 ; genOr
      0081D8 9E               [ 1]  215 	ld	a, xh
      0081D9 AA 20            [ 1]  216 	or	a, #0x20
      0081DB 95               [ 1]  217 	ld	xh, a
                                    218 ; genLabel
      0081DC                        219 00106$:
                                    220 ;	src/main.c: 41: if (p & 0x08) val |= SEG_D;
                                    221 ; genAnd
      0081DC 7B 01            [ 1]  222 	ld	a, (0x01, sp)
      0081DE A5 08            [ 1]  223 	bcp	a, #0x08
      0081E0 26 03            [ 1]  224 	jrne	00181$
      0081E2 CC 81 E9         [ 2]  225 	jp	00108$
      0081E5                        226 00181$:
                                    227 ; skipping generated iCode
                                    228 ; genOr
      0081E5 9E               [ 1]  229 	ld	a, xh
      0081E6 AA 40            [ 1]  230 	or	a, #0x40
      0081E8 95               [ 1]  231 	ld	xh, a
                                    232 ; genLabel
      0081E9                        233 00108$:
                                    234 ;	src/main.c: 42: if (p & 0x10) val |= SEG_E;
                                    235 ; genAnd
      0081E9 7B 01            [ 1]  236 	ld	a, (0x01, sp)
      0081EB A5 10            [ 1]  237 	bcp	a, #0x10
      0081ED 26 03            [ 1]  238 	jrne	00182$
      0081EF CC 81 F5         [ 2]  239 	jp	00110$
      0081F2                        240 00182$:
                                    241 ; skipping generated iCode
                                    242 ; genOr
      0081F2 58               [ 2]  243 	sllw	x
      0081F3 99               [ 1]  244 	scf
      0081F4 56               [ 2]  245 	rrcw	x
                                    246 ; genLabel
      0081F5                        247 00110$:
                                    248 ;	src/main.c: 44: GPIO_Write(SEG_PORT_C, (GPIO_ReadOutputData(SEG_PORT_C) & ~mask) | val);
                                    249 ; genSend
      0081F5 89               [ 2]  250 	pushw	x
      0081F6 AE 50 0A         [ 2]  251 	ldw	x, #0x500a
                                    252 ; genCall
      0081F9 CD 89 87         [ 4]  253 	call	_GPIO_ReadOutputData
      0081FC 6B 04            [ 1]  254 	ld	(0x04, sp), a
      0081FE 02               [ 1]  255 	rlwa	x
      0081FF 84               [ 1]  256 	pop	a
      008200 01               [ 1]  257 	rrwa	x
      008201 5B 01            [ 2]  258 	addw	sp, #1
                                    259 ; genCpl
      008203 A6 FF            [ 1]  260 	ld	a, #0xff
      008205 A8 F8            [ 1]  261 	xor	a, #0xf8
                                    262 ; genAnd
      008207 14 02            [ 1]  263 	and	a, (0x02, sp)
                                    264 ; genOr
      008209 89               [ 2]  265 	pushw	x
      00820A 1A 01            [ 1]  266 	or	a, (1, sp)
      00820C 85               [ 2]  267 	popw	x
                                    268 ; genSend
                                    269 ; genSend
      00820D AE 50 0A         [ 2]  270 	ldw	x, #0x500a
                                    271 ; genCall
      008210 CD 8A 7F         [ 4]  272 	call	_GPIO_Write
                                    273 ;	src/main.c: 46: if (p & 0x20) GPIO_WriteHigh(SEG_PORT_A, SEG_F);
                                    274 ; genAnd
      008213 7B 01            [ 1]  275 	ld	a, (0x01, sp)
      008215 A5 20            [ 1]  276 	bcp	a, #0x20
      008217 26 03            [ 1]  277 	jrne	00183$
      008219 CC 82 27         [ 2]  278 	jp	00112$
      00821C                        279 00183$:
                                    280 ; skipping generated iCode
                                    281 ; genSend
      00821C A6 02            [ 1]  282 	ld	a, #0x02
                                    283 ; genSend
      00821E AE 50 00         [ 2]  284 	ldw	x, #0x5000
                                    285 ; genCall
      008221 CD 8A 76         [ 4]  286 	call	_GPIO_WriteHigh
                                    287 ; genGoto
      008224 CC 82 2F         [ 2]  288 	jp	00113$
                                    289 ; genLabel
      008227                        290 00112$:
                                    291 ;	src/main.c: 47: else          GPIO_WriteLow(SEG_PORT_A, SEG_F);
                                    292 ; genSend
      008227 A6 02            [ 1]  293 	ld	a, #0x02
                                    294 ; genSend
      008229 AE 50 00         [ 2]  295 	ldw	x, #0x5000
                                    296 ; genCall
      00822C CD 89 48         [ 4]  297 	call	_GPIO_WriteLow
                                    298 ; genLabel
      00822F                        299 00113$:
                                    300 ;	src/main.c: 49: if (p & 0x40) GPIO_WriteHigh(SEG_PORT_A, SEG_G);
                                    301 ; genAnd
      00822F 7B 01            [ 1]  302 	ld	a, (0x01, sp)
      008231 A5 40            [ 1]  303 	bcp	a, #0x40
      008233 26 03            [ 1]  304 	jrne	00184$
      008235 CC 82 43         [ 2]  305 	jp	00115$
      008238                        306 00184$:
                                    307 ; skipping generated iCode
                                    308 ; genSend
      008238 A6 04            [ 1]  309 	ld	a, #0x04
                                    310 ; genSend
      00823A AE 50 00         [ 2]  311 	ldw	x, #0x5000
                                    312 ; genCall
      00823D CD 8A 76         [ 4]  313 	call	_GPIO_WriteHigh
                                    314 ; genGoto
      008240 CC 82 4B         [ 2]  315 	jp	00117$
                                    316 ; genLabel
      008243                        317 00115$:
                                    318 ;	src/main.c: 50: else          GPIO_WriteLow(SEG_PORT_A, SEG_G);
                                    319 ; genSend
      008243 A6 04            [ 1]  320 	ld	a, #0x04
                                    321 ; genSend
      008245 AE 50 00         [ 2]  322 	ldw	x, #0x5000
                                    323 ; genCall
      008248 CD 89 48         [ 4]  324 	call	_GPIO_WriteLow
                                    325 ; genLabel
      00824B                        326 00117$:
                                    327 ;	src/main.c: 51: }
                                    328 ; genEndFunction
      00824B 85               [ 2]  329 	popw	x
      00824C 85               [ 2]  330 	popw	x
      00824D 84               [ 1]  331 	pop	a
      00824E FC               [ 2]  332 	jp	(x)
                                    333 ;	src/main.c: 53: static void hw_init(void)
                                    334 ; genLabel
                                    335 ;	-----------------------------------------
                                    336 ;	 function hw_init
                                    337 ;	-----------------------------------------
                                    338 ;	Register assignment is optimal.
                                    339 ;	Stack space usage: 0 bytes.
      00824F                        340 _hw_init:
                                    341 ;	src/main.c: 55: CLK_HSIPrescalerConfig(CLK_PRESCALER_HSIDIV1);
                                    342 ; genSend
      00824F 4F               [ 1]  343 	clr	a
                                    344 ; genCall
      008250 CD 89 72         [ 4]  345 	call	_CLK_HSIPrescalerConfig
                                    346 ;	src/main.c: 56: init_milis();
                                    347 ; genCall
      008253 CD 85 EF         [ 4]  348 	call	_init_milis
                                    349 ;	src/main.c: 57: init_uart1(115200);
                                    350 ; genIPush
      008256 4B 00            [ 1]  351 	push	#0x00
      008258 4B C2            [ 1]  352 	push	#0xc2
      00825A 4B 01            [ 1]  353 	push	#0x01
      00825C 4B 00            [ 1]  354 	push	#0x00
                                    355 ; genCall
      00825E CD 86 43         [ 4]  356 	call	_init_uart1
                                    357 ;	src/main.c: 59: GPIO_Init(LED_PORT, LED_PIN, GPIO_MODE_OUT_PP_LOW_SLOW);
                                    358 ; genIPush
      008261 4B C0            [ 1]  359 	push	#0xc0
                                    360 ; genSend
      008263 A6 20            [ 1]  361 	ld	a, #0x20
                                    362 ; genSend
      008265 AE 50 05         [ 2]  363 	ldw	x, #0x5005
                                    364 ; genCall
      008268 CD 86 9E         [ 4]  365 	call	_GPIO_Init
                                    366 ;	src/main.c: 60: GPIO_Init(SEG_PORT_C, (GPIO_Pin_TypeDef)(SEG_A|SEG_B|SEG_C|SEG_D|SEG_E), GPIO_MODE_OUT_PP_LOW_SLOW);
                                    367 ; genIPush
      00826B 4B C0            [ 1]  368 	push	#0xc0
                                    369 ; genSend
      00826D A6 F8            [ 1]  370 	ld	a, #0xf8
                                    371 ; genSend
      00826F AE 50 0A         [ 2]  372 	ldw	x, #0x500a
                                    373 ; genCall
      008272 CD 86 9E         [ 4]  374 	call	_GPIO_Init
                                    375 ;	src/main.c: 61: GPIO_Init(SEG_PORT_A, (GPIO_Pin_TypeDef)(SEG_F|SEG_G), GPIO_MODE_OUT_PP_LOW_SLOW);
                                    376 ; genIPush
      008275 4B C0            [ 1]  377 	push	#0xc0
                                    378 ; genSend
      008277 A6 06            [ 1]  379 	ld	a, #0x06
                                    380 ; genSend
      008279 AE 50 00         [ 2]  381 	ldw	x, #0x5000
                                    382 ; genCall
      00827C CD 86 9E         [ 4]  383 	call	_GPIO_Init
                                    384 ;	src/main.c: 62: GPIO_Init(BTN_START_PORT, BTN_START_PIN, GPIO_MODE_IN_PU_NO_IT);
                                    385 ; genIPush
      00827F 4B 40            [ 1]  386 	push	#0x40
                                    387 ; genSend
      008281 A6 10            [ 1]  388 	ld	a, #0x10
                                    389 ; genSend
      008283 AE 50 05         [ 2]  390 	ldw	x, #0x5005
                                    391 ; genCall
      008286 CD 86 9E         [ 4]  392 	call	_GPIO_Init
                                    393 ;	src/main.c: 63: GPIO_Init(BTN_LAP_PORT,   BTN_LAP_PIN,  GPIO_MODE_IN_PU_NO_IT);
                                    394 ; genIPush
      008289 4B 40            [ 1]  395 	push	#0x40
                                    396 ; genSend
      00828B A6 80            [ 1]  397 	ld	a, #0x80
                                    398 ; genSend
      00828D AE 50 05         [ 2]  399 	ldw	x, #0x5005
                                    400 ; genCall
      008290 CD 86 9E         [ 4]  401 	call	_GPIO_Init
                                    402 ;	src/main.c: 65: seg7_show(0, false);
                                    403 ; genIPush
      008293 4B 00            [ 1]  404 	push	#0x00
                                    405 ; genSend
      008295 4F               [ 1]  406 	clr	a
                                    407 ; genCall
      008296 CD 81 9B         [ 4]  408 	call	_seg7_show
                                    409 ; genLabel
      008299                        410 00101$:
                                    411 ;	src/main.c: 66: }
                                    412 ; genEndFunction
      008299 81               [ 4]  413 	ret
                                    414 ;	src/main.c: 68: int main(void)
                                    415 ; genLabel
                                    416 ;	-----------------------------------------
                                    417 ;	 function main
                                    418 ;	-----------------------------------------
                                    419 ;	Register assignment might be sub-optimal.
                                    420 ;	Stack space usage: 93 bytes.
      00829A                        421 _main:
      00829A 52 5D            [ 2]  422 	sub	sp, #93
                                    423 ;	src/main.c: 70: hw_init();
                                    424 ; genCall
      00829C CD 82 4F         [ 4]  425 	call	_hw_init
                                    426 ;	src/main.c: 72: bool     running    = false;
                                    427 ; genAssign
      00829F 0F 2D            [ 1]  428 	clr	(0x2d, sp)
                                    429 ;	src/main.c: 73: uint32_t start_tick = 0;
                                    430 ; genAssign
      0082A1 5F               [ 1]  431 	clrw	x
      0082A2 1F 30            [ 2]  432 	ldw	(0x30, sp), x
      0082A4 1F 2E            [ 2]  433 	ldw	(0x2e, sp), x
                                    434 ;	src/main.c: 74: uint32_t elapsed    = 0;
                                    435 ; genAssign
      0082A6 5F               [ 1]  436 	clrw	x
      0082A7 1F 34            [ 2]  437 	ldw	(0x34, sp), x
      0082A9 1F 32            [ 2]  438 	ldw	(0x32, sp), x
                                    439 ;	src/main.c: 75: uint32_t lap        = 0;
                                    440 ; genAssign
      0082AB 5F               [ 1]  441 	clrw	x
      0082AC 1F 38            [ 2]  442 	ldw	(0x38, sp), x
      0082AE 1F 36            [ 2]  443 	ldw	(0x36, sp), x
                                    444 ;	src/main.c: 76: bool     lap_active = false;
                                    445 ; genAssign
      0082B0 0F 3A            [ 1]  446 	clr	(0x3a, sp)
                                    447 ;	src/main.c: 77: uint32_t lap_end    = 0;
                                    448 ; genAssign
      0082B2 5F               [ 1]  449 	clrw	x
      0082B3 1F 3D            [ 2]  450 	ldw	(0x3d, sp), x
      0082B5 1F 3B            [ 2]  451 	ldw	(0x3b, sp), x
                                    452 ;	src/main.c: 79: uint32_t led_tick   = 0;
                                    453 ; genAssign
      0082B7 5F               [ 1]  454 	clrw	x
      0082B8 1F 41            [ 2]  455 	ldw	(0x41, sp), x
      0082BA 1F 3F            [ 2]  456 	ldw	(0x3f, sp), x
                                    457 ;	src/main.c: 80: uint32_t uart_tick  = 0;
                                    458 ; genAssign
      0082BC 5F               [ 1]  459 	clrw	x
      0082BD 1F 45            [ 2]  460 	ldw	(0x45, sp), x
      0082BF 1F 43            [ 2]  461 	ldw	(0x43, sp), x
                                    462 ;	src/main.c: 81: uint32_t blink_tick = 0;
                                    463 ; genAssign
      0082C1 5F               [ 1]  464 	clrw	x
      0082C2 1F 49            [ 2]  465 	ldw	(0x49, sp), x
      0082C4 1F 47            [ 2]  466 	ldw	(0x47, sp), x
                                    467 ;	src/main.c: 82: bool     blink_on   = true;
                                    468 ; genAssign
      0082C6 A6 01            [ 1]  469 	ld	a, #0x01
      0082C8 6B 5D            [ 1]  470 	ld	(0x5d, sp), a
                                    471 ;	src/main.c: 84: bool     btn_s_prev = false,  btn_l_prev = false;
                                    472 ; genAssign
      0082CA 0F 4B            [ 1]  473 	clr	(0x4b, sp)
                                    474 ; genAssign
      0082CC 0F 4C            [ 1]  475 	clr	(0x4c, sp)
                                    476 ;	src/main.c: 85: uint32_t btn_s_t    = 0,      btn_l_t    = 0;
                                    477 ; genAssign
      0082CE 5F               [ 1]  478 	clrw	x
      0082CF 1F 4F            [ 2]  479 	ldw	(0x4f, sp), x
      0082D1 1F 4D            [ 2]  480 	ldw	(0x4d, sp), x
                                    481 ; genAssign
      0082D3 5F               [ 1]  482 	clrw	x
      0082D4 1F 53            [ 2]  483 	ldw	(0x53, sp), x
      0082D6 1F 51            [ 2]  484 	ldw	(0x51, sp), x
                                    485 ;	src/main.c: 88: while (1) {
                                    486 ; genLabel
      0082D8                        487 00134$:
                                    488 ;	src/main.c: 89: uint32_t now = milis();
                                    489 ; genCall
      0082D8 CD 85 CF         [ 4]  490 	call	_milis
      0082DB 1F 57            [ 2]  491 	ldw	(0x57, sp), x
      0082DD 17 55            [ 2]  492 	ldw	(0x55, sp), y
                                    493 ;	src/main.c: 91: bool s = (GPIO_ReadInputPin(BTN_START_PORT, BTN_START_PIN) == RESET);
                                    494 ; genSend
      0082DF A6 10            [ 1]  495 	ld	a, #0x10
                                    496 ; genSend
      0082E1 AE 50 05         [ 2]  497 	ldw	x, #0x5005
                                    498 ; genCall
      0082E4 CD 87 B1         [ 4]  499 	call	_GPIO_ReadInputPin
                                    500 ; genNot
      0082E7 A8 01            [ 1]  501 	xor	a, #0x01
      0082E9 6B 59            [ 1]  502 	ld	(0x59, sp), a
                                    503 ;	src/main.c: 92: bool l = (GPIO_ReadInputPin(BTN_LAP_PORT,   BTN_LAP_PIN)   == RESET);
                                    504 ; genSend
      0082EB A6 80            [ 1]  505 	ld	a, #0x80
                                    506 ; genSend
      0082ED AE 50 05         [ 2]  507 	ldw	x, #0x5005
                                    508 ; genCall
      0082F0 CD 87 B1         [ 4]  509 	call	_GPIO_ReadInputPin
                                    510 ; genNot
      0082F3 A8 01            [ 1]  511 	xor	a, #0x01
      0082F5 6B 5A            [ 1]  512 	ld	(0x5a, sp), a
                                    513 ;	src/main.c: 94: bool s_edge = s && !btn_s_prev && (now - btn_s_t > 20);
                                    514 ; genIfx
      0082F7 0D 59            [ 1]  515 	tnz	(0x59, sp)
      0082F9 26 03            [ 1]  516 	jrne	00297$
      0082FB CC 83 28         [ 2]  517 	jp	00138$
      0082FE                        518 00297$:
                                    519 ; genIfx
      0082FE 0D 4B            [ 1]  520 	tnz	(0x4b, sp)
      008300 27 03            [ 1]  521 	jreq	00298$
      008302 CC 83 28         [ 2]  522 	jp	00138$
      008305                        523 00298$:
                                    524 ; genMinus
      008305 1E 57            [ 2]  525 	ldw	x, (0x57, sp)
      008307 72 F0 4F         [ 2]  526 	subw	x, (0x4f, sp)
      00830A 1F 03            [ 2]  527 	ldw	(0x03, sp), x
      00830C 7B 56            [ 1]  528 	ld	a, (0x56, sp)
      00830E 12 4E            [ 1]  529 	sbc	a, (0x4e, sp)
      008310 6B 02            [ 1]  530 	ld	(0x02, sp), a
      008312 7B 55            [ 1]  531 	ld	a, (0x55, sp)
      008314 12 4D            [ 1]  532 	sbc	a, (0x4d, sp)
      008316 6B 01            [ 1]  533 	ld	(0x01, sp), a
                                    534 ; genCmp
                                    535 ; genCmpTnz
      008318 AE 00 14         [ 2]  536 	ldw	x, #0x0014
      00831B 13 03            [ 2]  537 	cpw	x, (0x03, sp)
      00831D 4F               [ 1]  538 	clr	a
      00831E 12 02            [ 1]  539 	sbc	a, (0x02, sp)
      008320 4F               [ 1]  540 	clr	a
      008321 12 01            [ 1]  541 	sbc	a, (0x01, sp)
      008323 24 03            [ 1]  542 	jrnc	00299$
      008325 CC 83 2C         [ 2]  543 	jp	00139$
      008328                        544 00299$:
                                    545 ; skipping generated iCode
                                    546 ; genLabel
      008328                        547 00138$:
                                    548 ; genAssign
      008328 4F               [ 1]  549 	clr	a
                                    550 ; genGoto
      008329 CC 83 2E         [ 2]  551 	jp	00140$
                                    552 ; genLabel
      00832C                        553 00139$:
                                    554 ; genAssign
      00832C A6 01            [ 1]  555 	ld	a, #0x01
                                    556 ; genLabel
      00832E                        557 00140$:
                                    558 ; genAssign
      00832E 6B 5B            [ 1]  559 	ld	(0x5b, sp), a
                                    560 ;	src/main.c: 95: bool l_edge = l && !btn_l_prev && (now - btn_l_t > 20);
                                    561 ; genIfx
      008330 0D 5A            [ 1]  562 	tnz	(0x5a, sp)
      008332 26 03            [ 1]  563 	jrne	00300$
      008334 CC 83 61         [ 2]  564 	jp	00144$
      008337                        565 00300$:
                                    566 ; genIfx
      008337 0D 4C            [ 1]  567 	tnz	(0x4c, sp)
      008339 27 03            [ 1]  568 	jreq	00301$
      00833B CC 83 61         [ 2]  569 	jp	00144$
      00833E                        570 00301$:
                                    571 ; genMinus
      00833E 1E 57            [ 2]  572 	ldw	x, (0x57, sp)
      008340 72 F0 53         [ 2]  573 	subw	x, (0x53, sp)
      008343 1F 03            [ 2]  574 	ldw	(0x03, sp), x
      008345 7B 56            [ 1]  575 	ld	a, (0x56, sp)
      008347 12 52            [ 1]  576 	sbc	a, (0x52, sp)
      008349 6B 02            [ 1]  577 	ld	(0x02, sp), a
      00834B 7B 55            [ 1]  578 	ld	a, (0x55, sp)
      00834D 12 51            [ 1]  579 	sbc	a, (0x51, sp)
      00834F 6B 01            [ 1]  580 	ld	(0x01, sp), a
                                    581 ; genCmp
                                    582 ; genCmpTnz
      008351 AE 00 14         [ 2]  583 	ldw	x, #0x0014
      008354 13 03            [ 2]  584 	cpw	x, (0x03, sp)
      008356 4F               [ 1]  585 	clr	a
      008357 12 02            [ 1]  586 	sbc	a, (0x02, sp)
      008359 4F               [ 1]  587 	clr	a
      00835A 12 01            [ 1]  588 	sbc	a, (0x01, sp)
      00835C 24 03            [ 1]  589 	jrnc	00302$
      00835E CC 83 65         [ 2]  590 	jp	00145$
      008361                        591 00302$:
                                    592 ; skipping generated iCode
                                    593 ; genLabel
      008361                        594 00144$:
                                    595 ; genAssign
      008361 4F               [ 1]  596 	clr	a
                                    597 ; genGoto
      008362 CC 83 67         [ 2]  598 	jp	00146$
                                    599 ; genLabel
      008365                        600 00145$:
                                    601 ; genAssign
      008365 A6 01            [ 1]  602 	ld	a, #0x01
                                    603 ; genLabel
      008367                        604 00146$:
                                    605 ; genAssign
      008367 6B 5C            [ 1]  606 	ld	(0x5c, sp), a
                                    607 ;	src/main.c: 97: if (!s) btn_s_prev = false;
                                    608 ; genIfx
      008369 0D 59            [ 1]  609 	tnz	(0x59, sp)
      00836B 27 03            [ 1]  610 	jreq	00303$
      00836D CC 83 75         [ 2]  611 	jp	00104$
      008370                        612 00303$:
                                    613 ; genAssign
      008370 0F 4B            [ 1]  614 	clr	(0x4b, sp)
                                    615 ; genGoto
      008372 CC 83 88         [ 2]  616 	jp	00105$
                                    617 ; genLabel
      008375                        618 00104$:
                                    619 ;	src/main.c: 98: else if (s_edge) { btn_s_prev = true; btn_s_t = now; }
                                    620 ; genIfx
      008375 0D 5B            [ 1]  621 	tnz	(0x5b, sp)
      008377 26 03            [ 1]  622 	jrne	00304$
      008379 CC 83 88         [ 2]  623 	jp	00105$
      00837C                        624 00304$:
                                    625 ; genAssign
      00837C A6 01            [ 1]  626 	ld	a, #0x01
      00837E 6B 4B            [ 1]  627 	ld	(0x4b, sp), a
                                    628 ; genAssign
      008380 16 57            [ 2]  629 	ldw	y, (0x57, sp)
      008382 17 4F            [ 2]  630 	ldw	(0x4f, sp), y
      008384 16 55            [ 2]  631 	ldw	y, (0x55, sp)
      008386 17 4D            [ 2]  632 	ldw	(0x4d, sp), y
                                    633 ; genLabel
      008388                        634 00105$:
                                    635 ;	src/main.c: 100: if (!l) btn_l_prev = false;
                                    636 ; genIfx
      008388 0D 5A            [ 1]  637 	tnz	(0x5a, sp)
      00838A 27 03            [ 1]  638 	jreq	00305$
      00838C CC 83 94         [ 2]  639 	jp	00109$
      00838F                        640 00305$:
                                    641 ; genAssign
      00838F 0F 4C            [ 1]  642 	clr	(0x4c, sp)
                                    643 ; genGoto
      008391 CC 83 A7         [ 2]  644 	jp	00110$
                                    645 ; genLabel
      008394                        646 00109$:
                                    647 ;	src/main.c: 101: else if (l_edge) { btn_l_prev = true; btn_l_t = now; }
                                    648 ; genIfx
      008394 0D 5C            [ 1]  649 	tnz	(0x5c, sp)
      008396 26 03            [ 1]  650 	jrne	00306$
      008398 CC 83 A7         [ 2]  651 	jp	00110$
      00839B                        652 00306$:
                                    653 ; genAssign
      00839B A6 01            [ 1]  654 	ld	a, #0x01
      00839D 6B 4C            [ 1]  655 	ld	(0x4c, sp), a
                                    656 ; genAssign
      00839F 16 57            [ 2]  657 	ldw	y, (0x57, sp)
      0083A1 17 53            [ 2]  658 	ldw	(0x53, sp), y
      0083A3 16 55            [ 2]  659 	ldw	y, (0x55, sp)
      0083A5 17 51            [ 2]  660 	ldw	(0x51, sp), y
                                    661 ; genLabel
      0083A7                        662 00110$:
                                    663 ;	src/main.c: 103: if (s_edge) {
                                    664 ; genIfx
      0083A7 0D 5B            [ 1]  665 	tnz	(0x5b, sp)
      0083A9 26 03            [ 1]  666 	jrne	00307$
      0083AB CC 83 F0         [ 2]  667 	jp	00115$
      0083AE                        668 00307$:
                                    669 ;	src/main.c: 104: if (!running) {
                                    670 ; genIfx
      0083AE 0D 2D            [ 1]  671 	tnz	(0x2d, sp)
      0083B0 27 03            [ 1]  672 	jreq	00308$
      0083B2 CC 83 CF         [ 2]  673 	jp	00112$
      0083B5                        674 00308$:
                                    675 ;	src/main.c: 105: start_tick = now - elapsed;
                                    676 ; genMinus
      0083B5 1E 57            [ 2]  677 	ldw	x, (0x57, sp)
      0083B7 72 F0 34         [ 2]  678 	subw	x, (0x34, sp)
      0083BA 1F 30            [ 2]  679 	ldw	(0x30, sp), x
      0083BC 7B 56            [ 1]  680 	ld	a, (0x56, sp)
      0083BE 12 33            [ 1]  681 	sbc	a, (0x33, sp)
      0083C0 6B 2F            [ 1]  682 	ld	(0x2f, sp), a
      0083C2 7B 55            [ 1]  683 	ld	a, (0x55, sp)
      0083C4 12 32            [ 1]  684 	sbc	a, (0x32, sp)
      0083C6 6B 2E            [ 1]  685 	ld	(0x2e, sp), a
                                    686 ;	src/main.c: 106: running    = true;
                                    687 ; genAssign
      0083C8 A6 01            [ 1]  688 	ld	a, #0x01
      0083CA 6B 2D            [ 1]  689 	ld	(0x2d, sp), a
                                    690 ; genGoto
      0083CC CC 83 F0         [ 2]  691 	jp	00115$
                                    692 ; genLabel
      0083CF                        693 00112$:
                                    694 ;	src/main.c: 108: elapsed    = now - start_tick;
                                    695 ; genMinus
      0083CF 1E 57            [ 2]  696 	ldw	x, (0x57, sp)
      0083D1 72 F0 30         [ 2]  697 	subw	x, (0x30, sp)
      0083D4 1F 34            [ 2]  698 	ldw	(0x34, sp), x
      0083D6 7B 56            [ 1]  699 	ld	a, (0x56, sp)
      0083D8 12 2F            [ 1]  700 	sbc	a, (0x2f, sp)
      0083DA 6B 33            [ 1]  701 	ld	(0x33, sp), a
      0083DC 7B 55            [ 1]  702 	ld	a, (0x55, sp)
      0083DE 12 2E            [ 1]  703 	sbc	a, (0x2e, sp)
      0083E0 6B 32            [ 1]  704 	ld	(0x32, sp), a
                                    705 ;	src/main.c: 109: running    = false;
                                    706 ; genAssign
      0083E2 0F 2D            [ 1]  707 	clr	(0x2d, sp)
                                    708 ;	src/main.c: 110: blink_tick = now;
                                    709 ; genAssign
      0083E4 16 57            [ 2]  710 	ldw	y, (0x57, sp)
      0083E6 17 49            [ 2]  711 	ldw	(0x49, sp), y
      0083E8 16 55            [ 2]  712 	ldw	y, (0x55, sp)
      0083EA 17 47            [ 2]  713 	ldw	(0x47, sp), y
                                    714 ;	src/main.c: 111: blink_on   = true;
                                    715 ; genAssign
      0083EC A6 01            [ 1]  716 	ld	a, #0x01
      0083EE 6B 5D            [ 1]  717 	ld	(0x5d, sp), a
                                    718 ; genLabel
      0083F0                        719 00115$:
                                    720 ;	src/main.c: 108: elapsed    = now - start_tick;
                                    721 ; genMinus
      0083F0 1E 57            [ 2]  722 	ldw	x, (0x57, sp)
      0083F2 72 F0 30         [ 2]  723 	subw	x, (0x30, sp)
      0083F5 1F 03            [ 2]  724 	ldw	(0x03, sp), x
      0083F7 7B 56            [ 1]  725 	ld	a, (0x56, sp)
      0083F9 12 2F            [ 1]  726 	sbc	a, (0x2f, sp)
      0083FB 97               [ 1]  727 	ld	xl, a
      0083FC 7B 55            [ 1]  728 	ld	a, (0x55, sp)
      0083FE 12 2E            [ 1]  729 	sbc	a, (0x2e, sp)
      008400 95               [ 1]  730 	ld	xh, a
                                    731 ;	src/main.c: 115: if (l_edge && running) {
                                    732 ; genIfx
      008401 0D 5C            [ 1]  733 	tnz	(0x5c, sp)
      008403 26 03            [ 1]  734 	jrne	00309$
      008405 CC 84 2D         [ 2]  735 	jp	00117$
      008408                        736 00309$:
                                    737 ; genIfx
      008408 0D 2D            [ 1]  738 	tnz	(0x2d, sp)
      00840A 26 03            [ 1]  739 	jrne	00310$
      00840C CC 84 2D         [ 2]  740 	jp	00117$
      00840F                        741 00310$:
                                    742 ;	src/main.c: 116: lap        = now - start_tick;
                                    743 ; genAssign
      00840F 1F 36            [ 2]  744 	ldw	(0x36, sp), x
      008411 16 03            [ 2]  745 	ldw	y, (0x03, sp)
      008413 17 38            [ 2]  746 	ldw	(0x38, sp), y
                                    747 ;	src/main.c: 117: lap_active = true;
                                    748 ; genAssign
      008415 A6 01            [ 1]  749 	ld	a, #0x01
      008417 6B 3A            [ 1]  750 	ld	(0x3a, sp), a
                                    751 ;	src/main.c: 118: lap_end    = now + 2000;
                                    752 ; genPlus
      008419 16 57            [ 2]  753 	ldw	y, (0x57, sp)
      00841B 72 A9 07 D0      [ 2]  754 	addw	y, #0x07d0
      00841F 17 3D            [ 2]  755 	ldw	(0x3d, sp), y
      008421 7B 56            [ 1]  756 	ld	a, (0x56, sp)
      008423 A9 00            [ 1]  757 	adc	a, #0x00
      008425 6B 3C            [ 1]  758 	ld	(0x3c, sp), a
      008427 7B 55            [ 1]  759 	ld	a, (0x55, sp)
      008429 A9 00            [ 1]  760 	adc	a, #0x00
      00842B 6B 3B            [ 1]  761 	ld	(0x3b, sp), a
                                    762 ; genLabel
      00842D                        763 00117$:
                                    764 ;	src/main.c: 121: if (running) elapsed = now - start_tick;
                                    765 ; genIfx
      00842D 0D 2D            [ 1]  766 	tnz	(0x2d, sp)
      00842F 26 03            [ 1]  767 	jrne	00311$
      008431 CC 84 3A         [ 2]  768 	jp	00120$
      008434                        769 00311$:
                                    770 ; genAssign
      008434 1F 32            [ 2]  771 	ldw	(0x32, sp), x
      008436 16 03            [ 2]  772 	ldw	y, (0x03, sp)
      008438 17 34            [ 2]  773 	ldw	(0x34, sp), y
                                    774 ; genLabel
      00843A                        775 00120$:
                                    776 ;	src/main.c: 122: if (lap_active && now >= lap_end) lap_active = false;
                                    777 ; genIfx
      00843A 0D 3A            [ 1]  778 	tnz	(0x3a, sp)
      00843C 26 03            [ 1]  779 	jrne	00312$
      00843E CC 84 54         [ 2]  780 	jp	00122$
      008441                        781 00312$:
                                    782 ; genCmp
                                    783 ; genCmpTnz
      008441 1E 57            [ 2]  784 	ldw	x, (0x57, sp)
      008443 13 3D            [ 2]  785 	cpw	x, (0x3d, sp)
      008445 7B 56            [ 1]  786 	ld	a, (0x56, sp)
      008447 12 3C            [ 1]  787 	sbc	a, (0x3c, sp)
      008449 7B 55            [ 1]  788 	ld	a, (0x55, sp)
      00844B 12 3B            [ 1]  789 	sbc	a, (0x3b, sp)
      00844D 24 03            [ 1]  790 	jrnc	00313$
      00844F CC 84 54         [ 2]  791 	jp	00122$
      008452                        792 00313$:
                                    793 ; skipping generated iCode
                                    794 ; genAssign
      008452 0F 3A            [ 1]  795 	clr	(0x3a, sp)
                                    796 ; genLabel
      008454                        797 00122$:
                                    798 ;	src/main.c: 124: uint32_t disp = lap_active ? lap : elapsed;
                                    799 ; genIfx
      008454 0D 3A            [ 1]  800 	tnz	(0x3a, sp)
      008456 26 03            [ 1]  801 	jrne	00314$
      008458 CC 84 64         [ 2]  802 	jp	00150$
      00845B                        803 00314$:
                                    804 ; genAssign
      00845B 16 38            [ 2]  805 	ldw	y, (0x38, sp)
      00845D 17 5B            [ 2]  806 	ldw	(0x5b, sp), y
      00845F 16 36            [ 2]  807 	ldw	y, (0x36, sp)
                                    808 ; genGoto
      008461 CC 84 6A         [ 2]  809 	jp	00151$
                                    810 ; genLabel
      008464                        811 00150$:
                                    812 ; genAssign
      008464 16 34            [ 2]  813 	ldw	y, (0x34, sp)
      008466 17 5B            [ 2]  814 	ldw	(0x5b, sp), y
      008468 16 32            [ 2]  815 	ldw	y, (0x32, sp)
                                    816 ; genLabel
      00846A                        817 00151$:
                                    818 ; genAssign
      00846A 1E 5B            [ 2]  819 	ldw	x, (0x5b, sp)
                                    820 ;	src/main.c: 125: uint8_t  dig  = (uint8_t)((disp / 1000) % 10);
                                    821 ; genIPush
      00846C 4B E8            [ 1]  822 	push	#0xe8
      00846E 4B 03            [ 1]  823 	push	#0x03
      008470 4B 00            [ 1]  824 	push	#0x00
      008472 4B 00            [ 1]  825 	push	#0x00
                                    826 ; genIPush
      008474 89               [ 2]  827 	pushw	x
      008475 90 89            [ 2]  828 	pushw	y
                                    829 ; genCall
      008477 CD 88 C0         [ 4]  830 	call	__divulong
      00847A 5B 08            [ 2]  831 	addw	sp, #8
                                    832 ; genIPush
      00847C 4B 0A            [ 1]  833 	push	#0x0a
      00847E 4B 00            [ 1]  834 	push	#0x00
      008480 4B 00            [ 1]  835 	push	#0x00
      008482 4B 00            [ 1]  836 	push	#0x00
                                    837 ; genIPush
      008484 89               [ 2]  838 	pushw	x
      008485 90 89            [ 2]  839 	pushw	y
                                    840 ; genCall
      008487 CD 87 BE         [ 4]  841 	call	__modulong
      00848A 5B 08            [ 2]  842 	addw	sp, #8
                                    843 ; genCast
                                    844 ; genAssign
                                    845 ;	src/main.c: 127: if (!running) {
                                    846 ; genIfx
      00848C 0D 2D            [ 1]  847 	tnz	(0x2d, sp)
      00848E 27 03            [ 1]  848 	jreq	00315$
      008490 CC 84 D2         [ 2]  849 	jp	00127$
      008493                        850 00315$:
                                    851 ;	src/main.c: 128: if (now - blink_tick > 500) {
                                    852 ; genMinus
      008493 16 57            [ 2]  853 	ldw	y, (0x57, sp)
      008495 72 F2 49         [ 2]  854 	subw	y, (0x49, sp)
      008498 17 5B            [ 2]  855 	ldw	(0x5b, sp), y
      00849A 7B 56            [ 1]  856 	ld	a, (0x56, sp)
      00849C 12 48            [ 1]  857 	sbc	a, (0x48, sp)
      00849E 6B 5A            [ 1]  858 	ld	(0x5a, sp), a
      0084A0 7B 55            [ 1]  859 	ld	a, (0x55, sp)
      0084A2 12 47            [ 1]  860 	sbc	a, (0x47, sp)
      0084A4 6B 59            [ 1]  861 	ld	(0x59, sp), a
                                    862 ; genCmp
                                    863 ; genCmpTnz
      0084A6 A6 F4            [ 1]  864 	ld	a, #0xf4
      0084A8 11 5C            [ 1]  865 	cp	a, (0x5c, sp)
      0084AA A6 01            [ 1]  866 	ld	a, #0x01
      0084AC 12 5B            [ 1]  867 	sbc	a, (0x5b, sp)
      0084AE 4F               [ 1]  868 	clr	a
      0084AF 12 5A            [ 1]  869 	sbc	a, (0x5a, sp)
      0084B1 4F               [ 1]  870 	clr	a
      0084B2 12 59            [ 1]  871 	sbc	a, (0x59, sp)
      0084B4 25 03            [ 1]  872 	jrc	00316$
      0084B6 CC 84 C6         [ 2]  873 	jp	00125$
      0084B9                        874 00316$:
                                    875 ; skipping generated iCode
                                    876 ;	src/main.c: 129: blink_on = !blink_on;
                                    877 ; genNot
      0084B9 04 5D            [ 1]  878 	srl	(0x5d, sp)
      0084BB 8C               [ 1]  879 	ccf
      0084BC 09 5D            [ 1]  880 	rlc	(0x5d, sp)
                                    881 ;	src/main.c: 130: blink_tick = now;
                                    882 ; genAssign
      0084BE 16 57            [ 2]  883 	ldw	y, (0x57, sp)
      0084C0 17 49            [ 2]  884 	ldw	(0x49, sp), y
      0084C2 16 55            [ 2]  885 	ldw	y, (0x55, sp)
      0084C4 17 47            [ 2]  886 	ldw	(0x47, sp), y
                                    887 ; genLabel
      0084C6                        888 00125$:
                                    889 ;	src/main.c: 132: seg7_show(dig, !blink_on);
                                    890 ; genNot
      0084C6 7B 5D            [ 1]  891 	ld	a, (0x5d, sp)
      0084C8 A8 01            [ 1]  892 	xor	a, #0x01
                                    893 ; genIPush
      0084CA 88               [ 1]  894 	push	a
                                    895 ; genSend
      0084CB 9F               [ 1]  896 	ld	a, xl
                                    897 ; genCall
      0084CC CD 81 9B         [ 4]  898 	call	_seg7_show
                                    899 ; genGoto
      0084CF CC 84 D8         [ 2]  900 	jp	00128$
                                    901 ; genLabel
      0084D2                        902 00127$:
                                    903 ;	src/main.c: 134: seg7_show(dig, false);
                                    904 ; genIPush
      0084D2 4B 00            [ 1]  905 	push	#0x00
                                    906 ; genSend
      0084D4 9F               [ 1]  907 	ld	a, xl
                                    908 ; genCall
      0084D5 CD 81 9B         [ 4]  909 	call	_seg7_show
                                    910 ; genLabel
      0084D8                        911 00128$:
                                    912 ;	src/main.c: 137: if (now - led_tick > 1000) {
                                    913 ; genMinus
      0084D8 1E 57            [ 2]  914 	ldw	x, (0x57, sp)
      0084DA 72 F0 41         [ 2]  915 	subw	x, (0x41, sp)
      0084DD 1F 5B            [ 2]  916 	ldw	(0x5b, sp), x
      0084DF 7B 56            [ 1]  917 	ld	a, (0x56, sp)
      0084E1 12 40            [ 1]  918 	sbc	a, (0x40, sp)
      0084E3 6B 5A            [ 1]  919 	ld	(0x5a, sp), a
      0084E5 7B 55            [ 1]  920 	ld	a, (0x55, sp)
      0084E7 12 3F            [ 1]  921 	sbc	a, (0x3f, sp)
      0084E9 6B 59            [ 1]  922 	ld	(0x59, sp), a
                                    923 ; genCmp
                                    924 ; genCmpTnz
      0084EB AE 03 E8         [ 2]  925 	ldw	x, #0x03e8
      0084EE 13 5B            [ 2]  926 	cpw	x, (0x5b, sp)
      0084F0 4F               [ 1]  927 	clr	a
      0084F1 12 5A            [ 1]  928 	sbc	a, (0x5a, sp)
      0084F3 4F               [ 1]  929 	clr	a
      0084F4 12 59            [ 1]  930 	sbc	a, (0x59, sp)
      0084F6 25 03            [ 1]  931 	jrc	00317$
      0084F8 CC 85 0B         [ 2]  932 	jp	00130$
      0084FB                        933 00317$:
                                    934 ; skipping generated iCode
                                    935 ;	src/main.c: 138: REVERSE(LED);
                                    936 ; genSend
      0084FB A6 20            [ 1]  937 	ld	a, #0x20
                                    938 ; genSend
      0084FD AE 50 05         [ 2]  939 	ldw	x, #0x5005
                                    940 ; genCall
      008500 CD 87 A8         [ 4]  941 	call	_GPIO_WriteReverse
                                    942 ;	src/main.c: 139: led_tick = now;
                                    943 ; genAssign
      008503 16 57            [ 2]  944 	ldw	y, (0x57, sp)
      008505 17 41            [ 2]  945 	ldw	(0x41, sp), y
      008507 16 55            [ 2]  946 	ldw	y, (0x55, sp)
      008509 17 3F            [ 2]  947 	ldw	(0x3f, sp), y
                                    948 ; genLabel
      00850B                        949 00130$:
                                    950 ;	src/main.c: 142: if (now - uart_tick > 1000) {
                                    951 ; genMinus
      00850B 1E 57            [ 2]  952 	ldw	x, (0x57, sp)
      00850D 72 F0 45         [ 2]  953 	subw	x, (0x45, sp)
      008510 1F 5B            [ 2]  954 	ldw	(0x5b, sp), x
      008512 7B 56            [ 1]  955 	ld	a, (0x56, sp)
      008514 12 44            [ 1]  956 	sbc	a, (0x44, sp)
      008516 6B 5A            [ 1]  957 	ld	(0x5a, sp), a
      008518 7B 55            [ 1]  958 	ld	a, (0x55, sp)
      00851A 12 43            [ 1]  959 	sbc	a, (0x43, sp)
      00851C 6B 59            [ 1]  960 	ld	(0x59, sp), a
                                    961 ; genCmp
                                    962 ; genCmpTnz
      00851E AE 03 E8         [ 2]  963 	ldw	x, #0x03e8
      008521 13 5B            [ 2]  964 	cpw	x, (0x5b, sp)
      008523 4F               [ 1]  965 	clr	a
      008524 12 5A            [ 1]  966 	sbc	a, (0x5a, sp)
      008526 4F               [ 1]  967 	clr	a
      008527 12 59            [ 1]  968 	sbc	a, (0x59, sp)
      008529 25 03            [ 1]  969 	jrc	00318$
      00852B CC 82 D8         [ 2]  970 	jp	00134$
      00852E                        971 00318$:
                                    972 ; skipping generated iCode
                                    973 ;	src/main.c: 145: (uint16_t)(lap / 1000),     (uint8_t)((lap % 1000) / 10));
                                    974 ; genIPush
      00852E 4B E8            [ 1]  975 	push	#0xe8
      008530 4B 03            [ 1]  976 	push	#0x03
      008532 5F               [ 1]  977 	clrw	x
      008533 89               [ 2]  978 	pushw	x
                                    979 ; genIPush
      008534 1E 3C            [ 2]  980 	ldw	x, (0x3c, sp)
      008536 89               [ 2]  981 	pushw	x
      008537 1E 3C            [ 2]  982 	ldw	x, (0x3c, sp)
      008539 89               [ 2]  983 	pushw	x
                                    984 ; genCall
      00853A CD 87 BE         [ 4]  985 	call	__modulong
      00853D 5B 08            [ 2]  986 	addw	sp, #8
                                    987 ; genIPush
      00853F 4B 0A            [ 1]  988 	push	#0x0a
      008541 4B 00            [ 1]  989 	push	#0x00
      008543 4B 00            [ 1]  990 	push	#0x00
      008545 4B 00            [ 1]  991 	push	#0x00
                                    992 ; genIPush
      008547 89               [ 2]  993 	pushw	x
      008548 90 89            [ 2]  994 	pushw	y
                                    995 ; genCall
      00854A CD 88 C0         [ 4]  996 	call	__divulong
      00854D 5B 08            [ 2]  997 	addw	sp, #8
      00854F 9F               [ 1]  998 	ld	a, xl
                                    999 ; genCast
                                   1000 ; genAssign
                                   1001 ; genIPush
      008550 88               [ 1] 1002 	push	a
      008551 4B E8            [ 1] 1003 	push	#0xe8
      008553 4B 03            [ 1] 1004 	push	#0x03
      008555 5F               [ 1] 1005 	clrw	x
      008556 89               [ 2] 1006 	pushw	x
                                   1007 ; genIPush
      008557 1E 3D            [ 2] 1008 	ldw	x, (0x3d, sp)
      008559 89               [ 2] 1009 	pushw	x
      00855A 1E 3D            [ 2] 1010 	ldw	x, (0x3d, sp)
      00855C 89               [ 2] 1011 	pushw	x
                                   1012 ; genCall
      00855D CD 88 C0         [ 4] 1013 	call	__divulong
      008560 5B 08            [ 2] 1014 	addw	sp, #8
      008562 84               [ 1] 1015 	pop	a
                                   1016 ; genCast
                                   1017 ; genAssign
      008563 1F 5A            [ 2] 1018 	ldw	(0x5a, sp), x
                                   1019 ;	src/main.c: 144: (uint16_t)(elapsed / 1000), (uint8_t)((elapsed % 1000) / 10),
                                   1020 ; genIPush
      008565 88               [ 1] 1021 	push	a
      008566 4B E8            [ 1] 1022 	push	#0xe8
      008568 4B 03            [ 1] 1023 	push	#0x03
      00856A 5F               [ 1] 1024 	clrw	x
      00856B 89               [ 2] 1025 	pushw	x
                                   1026 ; genIPush
      00856C 1E 39            [ 2] 1027 	ldw	x, (0x39, sp)
      00856E 89               [ 2] 1028 	pushw	x
      00856F 1E 39            [ 2] 1029 	ldw	x, (0x39, sp)
      008571 89               [ 2] 1030 	pushw	x
                                   1031 ; genCall
      008572 CD 87 BE         [ 4] 1032 	call	__modulong
      008575 5B 08            [ 2] 1033 	addw	sp, #8
      008577 1F 46            [ 2] 1034 	ldw	(0x46, sp), x
      008579 84               [ 1] 1035 	pop	a
                                   1036 ; genIPush
      00857A 88               [ 1] 1037 	push	a
      00857B 4B 0A            [ 1] 1038 	push	#0x0a
      00857D 5F               [ 1] 1039 	clrw	x
      00857E 89               [ 2] 1040 	pushw	x
      00857F 4B 00            [ 1] 1041 	push	#0x00
                                   1042 ; genIPush
      008581 1E 4A            [ 2] 1043 	ldw	x, (0x4a, sp)
      008583 89               [ 2] 1044 	pushw	x
      008584 90 89            [ 2] 1045 	pushw	y
                                   1046 ; genCall
      008586 CD 88 C0         [ 4] 1047 	call	__divulong
      008589 5B 08            [ 2] 1048 	addw	sp, #8
      00858B 84               [ 1] 1049 	pop	a
                                   1050 ; genCast
                                   1051 ; genAssign
      00858C 41               [ 1] 1052 	exg	a, xl
      00858D 6B 5C            [ 1] 1053 	ld	(0x5c, sp), a
      00858F 41               [ 1] 1054 	exg	a, xl
                                   1055 ; genIPush
      008590 88               [ 1] 1056 	push	a
      008591 4B E8            [ 1] 1057 	push	#0xe8
      008593 4B 03            [ 1] 1058 	push	#0x03
      008595 5F               [ 1] 1059 	clrw	x
      008596 89               [ 2] 1060 	pushw	x
                                   1061 ; genIPush
      008597 1E 39            [ 2] 1062 	ldw	x, (0x39, sp)
      008599 89               [ 2] 1063 	pushw	x
      00859A 1E 39            [ 2] 1064 	ldw	x, (0x39, sp)
      00859C 89               [ 2] 1065 	pushw	x
                                   1066 ; genCall
      00859D CD 88 C0         [ 4] 1067 	call	__divulong
      0085A0 5B 08            [ 2] 1068 	addw	sp, #8
      0085A2 84               [ 1] 1069 	pop	a
                                   1070 ; genCast
                                   1071 ; genAssign
                                   1072 ;	src/main.c: 143: sprintf(buf, "T=%u.%02us LAP=%u.%02us\r\n",
                                   1073 ; skipping iCode since result will be rematerialized
                                   1074 ; skipping iCode since result will be rematerialized
                                   1075 ; skipping iCode since result will be rematerialized
                                   1076 ; skipping iCode since result will be rematerialized
                                   1077 ; genIPush
      0085A3 88               [ 1] 1078 	push	a
                                   1079 ; genIPush
      0085A4 16 5B            [ 2] 1080 	ldw	y, (0x5b, sp)
      0085A6 90 89            [ 2] 1081 	pushw	y
                                   1082 ; genIPush
      0085A8 7B 5F            [ 1] 1083 	ld	a, (0x5f, sp)
      0085AA 88               [ 1] 1084 	push	a
                                   1085 ; genIPush
      0085AB 89               [ 2] 1086 	pushw	x
                                   1087 ; genIPush
      0085AC 4B 9F            [ 1] 1088 	push	#<(___str_0+0)
      0085AE 4B 80            [ 1] 1089 	push	#((___str_0+0) >> 8)
                                   1090 ; genIPush
      0085B0 96               [ 1] 1091 	ldw	x, sp
      0085B1 1C 00 0D         [ 2] 1092 	addw	x, #13
      0085B4 89               [ 2] 1093 	pushw	x
                                   1094 ; genCall
      0085B5 CD 88 53         [ 4] 1095 	call	_sprintf
      0085B8 5B 0A            [ 2] 1096 	addw	sp, #10
                                   1097 ;	src/main.c: 146: uart1_puts(buf);
                                   1098 ; skipping iCode since result will be rematerialized
                                   1099 ; skipping iCode since result will be rematerialized
                                   1100 ; genSend
      0085BA 96               [ 1] 1101 	ldw	x, sp
      0085BB 1C 00 05         [ 2] 1102 	addw	x, #5
                                   1103 ; genCall
      0085BE CD 86 6A         [ 4] 1104 	call	_uart1_puts
                                   1105 ;	src/main.c: 147: uart_tick = now;
                                   1106 ; genAssign
      0085C1 16 57            [ 2] 1107 	ldw	y, (0x57, sp)
      0085C3 17 45            [ 2] 1108 	ldw	(0x45, sp), y
      0085C5 16 55            [ 2] 1109 	ldw	y, (0x55, sp)
      0085C7 17 43            [ 2] 1110 	ldw	(0x43, sp), y
                                   1111 ; genGoto
      0085C9 CC 82 D8         [ 2] 1112 	jp	00134$
                                   1113 ; genLabel
      0085CC                       1114 00136$:
                                   1115 ;	src/main.c: 150: }
                                   1116 ; genEndFunction
      0085CC 5B 5D            [ 2] 1117 	addw	sp, #93
      0085CE 81               [ 4] 1118 	ret
                                   1119 	.area CODE
                                   1120 	.area CONST
      008095                       1121 _SEG7:
      008095 3F                    1122 	.db #0x3f	; 63
      008096 06                    1123 	.db #0x06	; 6
      008097 5B                    1124 	.db #0x5b	; 91
      008098 4F                    1125 	.db #0x4f	; 79	'O'
      008099 66                    1126 	.db #0x66	; 102	'f'
      00809A 6D                    1127 	.db #0x6d	; 109	'm'
      00809B 7D                    1128 	.db #0x7d	; 125
      00809C 07                    1129 	.db #0x07	; 7
      00809D 7F                    1130 	.db #0x7f	; 127
      00809E 6F                    1131 	.db #0x6f	; 111	'o'
                                   1132 	.area CONST
      00809F                       1133 ___str_0:
      00809F 54 3D 25 75 2E 25 30  1134 	.ascii "T=%u.%02us LAP=%u.%02us"
             32 75 73 20 4C 41 50
             3D 25 75 2E 25 30 32
             75 73
      0080B6 0D                    1135 	.db 0x0d
      0080B7 0A                    1136 	.db 0x0a
      0080B8 00                    1137 	.db 0x00
                                   1138 	.area CODE
                                   1139 	.area INITIALIZER
                                   1140 	.area CABS (ABS)
