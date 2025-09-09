3-bit Gray Counter (D-Flip-Flop Implementation) — matching your diagram

Physical FF mapping in your diagram

Lowest (bottom) flip-flop → D0, Q0, Q̅0 (LSB)

Middle flip-flop → D1, Q1, Q̅1

Upper (top) flip-flop → Q2, D2, Q̅2 (MSB)

One-line description
A synchronous 3-bit Gray code counter implemented with three D flip-flops (bottom→Q0, middle→Q1, top→Q2) and simple combinational logic so that exactly one bit toggles per clock.

I/O (suggested / typical wiring)

Clock → common CLK to all three D-FFs.

Reset (sync or async) → forces Q2 Q1 Q0 = 000.

Enable → gate counting (when 0 hold state; when 1 advance).

Outputs: connect LEDs to Q0 (bottom), Q1 (middle), Q2 (top).

Next-state Boolean logic (drive D inputs)

Use these equations to wire the combinational logic feeding the D inputs (consistent with Gray sequence 000→001→011→010→110→111→101→100→000):

D0 = (NOTQ0 NOTQ1) + (Q2 Q1)

D1 = (NOTQ2 Q0) + (Q1 NOTQ0)

D2 = (Q2 Q0) + (Q1 NOTQ0)

(Implement NOT, XOR, and one AND + XOR for the MSB.)

Truth table (current Q2 Q1 Q0 → next D2 D1 D0)
Q2	Q1	Q0	D2	D1	D0
0	  0	  0	  0	  0  	1
0	  0	  1	  0	  1	  1
0	  1	  0	  1	  1	  0
0	  1	  1	  0	  1	  0
1	  1	  0	  1	  1	  1
1	  1	  1	  1	  0	  1
1	  0	  1	  1	  0	  0
1	  0	  0	  0	  0	  0

(Each row: current state → inputs that should be presented at D2,D1,D0 before the next rising clock.)

Example counting sequence (observed on Q2 Q1 Q0 top→bottom)

000 → 001 → 011 → 010 → 110 → 111 → 101 → 100 → (back to) 000
Only one bit changes between consecutive states.

Implementation notes (matching your schematic)

Bottom FF output = Q0 → connect to D0 logic: feed Q0 through an inverter to get D0.

Middle FF output = Q1 → feed Q1 and Q0 into XOR gate to produce D1.

Top FF output = Q2 → produce D2 by XORing Q2 with the AND of Q1 and Q0 (D2 = Q2 XOR (Q1 & Q0)).

Use a common clock to the CLK pins on all three D-FFs.

If you have Enable, implement: when Enable=0 force D = Q (hold) — simplest is multiplex each D between next_logic and Q controlled by Enable.

If you have Reset, tie it to FF async or implement synchronous clear to force 000.
