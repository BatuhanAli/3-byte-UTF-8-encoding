.data
code_point: .word 0x8A9E    # Your code point in register $a3

.text
main:
    lw   $a3, code_point  # Load the code point into register $a3

    # Encode the code point into a 3-byte UTF-8 sequence
    li   $t2, 0xE0        # First byte header: 1110xxxx
    li   $t3, 0x80        # Second and Third byte header: 10xxxxxx

    srl  $t4, $a3, 12     # Extract the first 4 bits
    andi $t4, $t4, 0x0F   # Ensure only the lowest 4 bits are considered

    srl  $t5, $a3, 6      # Extract the next 6 bits
    andi $t5, $t5, 0x3F   # Ensure only the lowest 6 bits are considered

    srl  $t6, $a3, 0      # Extract the lowest 6 bits
    andi $t6, $t6, 0x3F   # Ensure only the lowest 6 bits are considered

    or   $t2, $t2, $t4    # Combine the bits for the first byte
    or   $t7, $t3, $t5    # Combine the bits for the second byte
    or   $t8, $t3, $t6    # Combine the bits for the third byte

    sll  $a2, $t8, 8
    or   $a2, $a2, $t7
    sll  $a2, $a2, 8
    or   $a2, $a2, $t2



    # Exit the program
    li   $v0, 10           # Exit program
    syscall                 