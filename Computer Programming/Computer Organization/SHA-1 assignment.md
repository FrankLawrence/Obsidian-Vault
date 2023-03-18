.text

.global sha1_chunk
.global main_loop
.global first_if
.global second_if
.global third_if
.global fourth_if
.global loop_done
.global end_main

.equ H0, 0x67452301
.equ H1, 0xEFCDAB89
.equ H2, 0x98BADCFE
.equ H3, 0x10325476
.equ H4, 0xC3D2E1F0

.equ K1, 0x5A827999
.equ K2, 0x6ED9EBA1
.equ K3, 0x8F1BBCDC
.equ K4, 0xCA62C1D6

sha1_chunk:
        # pushing the caller saved values and initialising rcx
        push    %r15 # = loop
        push    %r14 # = temp
        push    %r13 # = k
        push    %r10 # = f
        push    %rbx
        movq    $16, %rcx
loop_w:
        # w[i] = (w[i-3] xor w[i-8] xor w[i-14] xor w[i-16]) leftrotate 1
        subq    $3, %rcx
        movl    (%rsi, %rcx, 4), %edx # = w[i-3]
        subq    $5, %rcx
        movl    (%rsi, %rcx, 4), %r8d # = w[i-8]
        xor     %r8, %rdx
        subq    $6, %rcx
        movl    (%rsi, %rcx, 4), %r8d # = w[i-14]
        xor     %r8, %rdx
        subq    $2, %rcx
        movl    (%rsi, %rcx, 4), %r8d # = w[i-16]
        xor     %r8, %rdx
        rol     $1, %edx
        addq    $16, %rcx
        movl    %edx, (%rsi, %rcx, 4)
        incq    %rcx
        cmpq    $80, %rcx
        jne     loop_w

        #get rdi array values and place them inside registers (32 bits only)
        movq    $0, %rax
        movl    (%rdi, %rax, 4), %ebx # = a
        incq    %rax
        movl    (%rdi, %rax, 4), %ecx # = b
        incq    %rax
        movl    (%rdi, %rax, 4), %edx # = c
        incq    %rax
        movl    (%rdi, %rax, 4), %r8d # = d
        incq    %rax
        movl    (%rdi, %rax, 4), %r9d # = e

        # initialise %r15 and jump to main
        movq    $0, %r15
        jmp    main_loop

main_loop:
        # check main loop value r15 to find which branch to jump to
        cmpq    $19, %r15
        jle     first_if
        cmpq    $39, %r15
        jle     second_if
        cmpq    $59, %r15
        jle     third_if
        cmpq    $79, %r15
        jle     fourth_if
        jmp     loop_done

first_if:

        # %r10 = %r8 ^ (%rcx & (%rdx ^ %r8))
        movl    %edx, %r10d
        xor     %r8d, %r10d
        and     %ecx, %r10d
        xor     %r8d, %r10d

        # Setting K (%r13) and %r10
        # movq    %r14, %r10
        movq    $K1, %r13

        jmp     end_main_loop

second_if:
        # %r10 = %rcx xor %rdx xor %r8
        movq    %rcx, %r14
        xor     %rdx, %r14
        xor     %r8, %r14

        # Setting K (%r13) and %r10
        movq    %r14, %r10
        movq    $K2, %r13
        
        jmp     end_main_loop

third_if:

        # %r10 = (%rcx and %rdx) xor (%rcx and %r8) xor (%rdx and %r8)
        movq    %rcx, %r14
        and     %rdx, %r14 # r14 = (%rcx and %rdx)
        movq    %rcx, %r13
        and     %r8, %r13 # r13 = (%rcx and %r8)
        xor     %r14, %r13 # r13 = (%rcx and %rdx) xor (%rcx and %r8)
        movq    %rdx, %r14 
        and     %r8, %r14  # r14 = (%rdx and %r8)
        xor     %r13, %r14

        # Setting K (%r13) and %r10
        movq    %r14, %r10
        movq    $K3, %r13
        jmp     end_main_loop

fourth_if:

        # %r10 = %rcx xor %rdx xor %r8
        movq    %rcx, %r14
        xor     %rdx, %r14
        xor     %r8, %r14

        # Setting K (%r13) and %r10
        movq    %r14, %r10
        movq    $K4, %r13
        jmp     end_main_loop

end_main_loop:
        # %r14 = (%rdi leftrotate 5) + %r10 + %r9 + %r13 + w[i]
        movq    $0, %r14
        push    %rbx
        roll    $5, %ebx
        addq    %rbx, %r14
        addq    %r10, %r14
        addq    %r9, %r14
        addq    %r13, %r14
        movq    (%rsi, %r15, 4), %rax
        addq    (%rsi, %r15, 4), %r14 # = + w[i]
        pop     %rbx

        movq    %r8, %r9
        movq    %rdx, %r8

        # %rdx = %rcx leftrotate 30
        push    %r10
        movq    %rcx, %r10
        roll    $30, %r10d
        movq    %r10, %rdx
        pop     %r10

        movq    %rbx, %rcx
        movl    %r14d, %ebx
        incq    %r15
        jmp     main_loop
loop_done:
        movq    $0, %r15
        addl    %ebx, (%rdi, %r15, 4) # = a
        incq    %r15
        addl    %ecx, (%rdi, %r15, 4) # = b
        incq    %r15
        addl    %edx, (%rdi, %r15, 4) # = c
        incq    %r15
        addl    %r8d, (%rdi, %r15, 4) # = d
        incq    %r15
        addl    %r9d, (%rdi, %r15, 4) # = e

        # reassigning the caller saved values
        pop     %rbx
        pop     %r10
        pop     %r13
        pop     %r14
        pop     %r15
        ret