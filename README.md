; A simple template for assembly programs.
.386  ; Specify instruction set
.model flat, stdcall  ; Flat memory model, std. calling convention
.stack 4096 ; Reserve stack space
ExitProcess PROTO, dwExitCode: DWORD  ; Exit process prototype

.data ; data segment
	arrayA DWORD 2,2,3,1,7,5,0,8,9,2
	arrayB DWORD 10 DUP (0)
	ArrayC DWORD 1,3,2,5,4,6,0,4,5,8

	equation1 DWORD 0
	equation2 DWORD 0
	equation3 DWORD 0

.code ; code segment

main PROC ; main procedure
	mov ebx, 2; load multiplier
	mov ecx, 3; load divisor
	mov esi, 0

update:
	mov eax, [arrayA + TYPE arrayA * esi]

	add eax, 1
	mov equation1, eax

	mov eax, [arrayC + TYPE arrayC * esi]
	mul ebx ;C[i]*2
	add eax, 1 ;C[i]*2 + 1
	mov equation2, eax

	mov eax, [arrayA + TYPE arrayA * esi]
	mov edx, [arrayC + TYPE arrayC * esi]
	add eax, edx ; eax = A[i] + C[i] 

	mov edx, 0

	div ecx; (A[i] + C[i]) / 3

	add eax, equation1
	add eax, equation2

	mov [arrayB + TYPE arrayB * esi], eax

	inc esi
	cmp esi, 10
	jne update

	INVOKE ExitProcess, 0 ; call exit function
  
main ENDP ; exit main procedure
END main  ; stop assembling
