.data
    promptL:    .asciiz "Enter the value of L (> 20): "
    promptM:    .asciiz "Enter the value of M (> 20): "
    promptN:    .asciiz "Enter the value of N (> 20): "
    error:      .asciiz "Illegal Number!\n"

.text
    # Function to calculate GCD
    gcd:
        # Inputs: $a0 = first number, $a1 = second number
        # Outputs: $v0 = GCD
        # Temp registers: $t0, $t1

   move $t0, $a0         # Copy first number to $t0
   move $t1, $a1         # Copy second number to $t1

  gcd_loop:
        beq $t1, $zero, gcd_done   # If $t1 is 0, GCD is in $t0, exit loop
        rem $t2, $t0, $t1          # $t2 = remainder of $t0 divided by $t1
        move $t0, $t1              # $t0 = $t1
        move $t1, $t2              # $t1 = $t2
        j gcd_loop

  gcd_done:
        move $v0, $t0              # GCD is in $v0
        jr $ra

  # Main program
   main:
        # Prompt for L
    input_L:
        li $v0, 4
        la $a0, promptL
        syscall
        li $v0, 5
        syscall
        blt $v0, 21, input_L     # Check if L is greater than 20
        move $s0, $v0            # Save L in $s0

  # Prompt for M
  input_M:
        li $v0, 4
        la $a0, promptM
        syscall
        li $v0, 5
        syscall
        blt $v0, 21, input_M     # Check if M is greater than 20
        move $s1, $v0            # Save M in $s1

  # Prompt for N
   input_N:
        li $v0, 4
        la $a0, promptN
        syscall
        li $v0, 5
        syscall
        blt $v0, 21, input_N     # Check if N is greater than 20
        move $s2, $v0            # Save N in $s2

  # Check for illegal numbers
   blt $s0, 1, illegal_error
   blt $s1, 1, illegal_error
   blt $s2, 1, illegal_error
   j calculate_gcd

  illegal_error:
        li $v0, 4
        la $a0, error
        syscall
        j main

   calculate_gcd:
        # Calculate GCD of L, M, and N
        move $a0, $s0
        move $a1, $s1
        jal gcd
        move $s3, $v0  # Save GCD in $s3

  # Display the result
  li $v0, 1
        move $a0, $s3
        syscall

   # Exit the program
  li $v0, 10
  syscall
