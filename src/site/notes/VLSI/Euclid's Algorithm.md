---
{"dg-publish":true,"permalink":"/vlsi/euclid-s-algorithm/"}
---

```verilog
module gcd (
    input [31:0] X, 
    input [31:0] Y, 
    output reg [31:0] GCD 
);
    reg [31:0] a, b;
    
    always @(*) begin
        a = X; 
        b = Y; 
        
        while (b != 0) begin
            if (a < b) begin
                // Swap a and b
                a = a + b;
                b = a - b;
                a = a - b;
            end
            a = a - b;  
        end
        
        GCD = a;  // When b becomes 0, a is the GCD
    end
endmodule
```

### Test Bench:

```verilog
module tb_gcd();
    reg [31:0] X, Y;       
    wire [31:0] GCD;        
    
    gcd uut (
        .X(X),
        .Y(Y),
        .GCD(GCD)
    );
    
    initial begin

        X = 56; 
        Y = 98;
        #10;  
        $display("Test Case 1: GCD of %d and %d is %d", X, Y, GCD);
        

        X = 1071; 
        Y = 462;
        #10;
        $display("Test Case 2: GCD of %d and %d is %d", X, Y, GCD);
        

        X = 48; 
        Y = 18;
        #10;
        $display("Test Case 3: GCD of %d and %d is %d", X, Y, GCD);
        

        X = 0; 
        Y = 25;
        #10;
        $display("Test Case 4: GCD of %d and %d is %d", X, Y, GCD);


        X = 37; 
        Y = 600;
        #10;
        $display("Test Case 5: GCD of %d and %d is %d", X, Y, GCD);
        
        X = 101; 
        Y = 101;
        #10;
        $display("Test Case 6: GCD of %d and %d is %d", X, Y, GCD);
        X = 1234; 
        Y = 0;
        #10;
        $display("Test Case 7: GCD of %d and %d is %d", X, Y, GCD);
        
        $finish; 
    end
endmodule
```

