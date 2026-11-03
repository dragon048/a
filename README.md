; A simple template for assembly programs.
.386  ; Specify instruction set
.model flat, stdcall  ; Flat memory model, std. calling convention
.stack 4096 ; Reserve stack space
ExitProcess PROTO, dwExitCode: DWORD  ; Exit process prototype

.data ; data segment
	arrayA DWORD 2,2,3,1,7,5,0,8,9,2; initialise array A
	arrayB DWORD 10 DUP (0) ; initialise empty array B
	ArrayC DWORD 1,3,2,5,4,6,0,4,5,8 ; initialise array C
	

	equationA DWORD 0
	equationB DWORD 0
	equationC DWORD 0


.code ; code segment

main PROC ; main procedure
	mov ebx, 2; load multiplier from c[i]*2 + 1
	mov ecx, 3; load divisor
	mov esi, 0; load 0 as this will be used to increment in the loop

myloop:
	mov eax, [arrayA + TYPE arrayA * esi] ; take the nth element which is decided by the value of esi

	add eax, 1
	mov equationA, eax

	mov eax, [arrayC + TYPE arrayC * esi]
	mul ebx ;C[i]*2
	add eax, 1 ;C[i]*2 + 1
	mov equationB, eax

	mov eax, [arrayA + TYPE arrayA * esi]
	mov edx, [arrayC + TYPE arrayC * esi]
	add eax, edx ; eax = A[i] + C[i] 


	mov edx, 0; zero out for the division

	div ecx; (A[i] + C[i]) / 3

	add eax, equationA
	add eax, equationB

	mov [arrayB + TYPE arrayB * esi], eax

	inc esi
	cmp esi, 10
	jne myloop

	INVOKE ExitProcess, 0 ; call exit function
  
main ENDP ; exit main procedure
END main  ; stop assembling
