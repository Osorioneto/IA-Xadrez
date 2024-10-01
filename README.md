# IA-Xadrez

#Criado por Igor Vinicius e Vicente Osóri - Chess Challenge (Clube da Nénem)
#Bibliotecas Py Usadas: Chess
#Motor IA Externo usado: StockFish

import chess
import chess.engine

def print_board(board):
    print(board)

def display_board(board):
    print("  a b c d e f g h")
    print(" +----------------")
    for rank in range(8, 0, -1):
        print(f"{rank}|", end=" ")
        for file in range(1, 9):
            square = chess.square(file - 1, rank - 1)
            piece = board.piece_at(square)
            if piece is None:
                print(". ", end="")
            else:
                print(piece.symbol(), end=" ")
        print("|")
    print(" +----------------")

def main():
    stockfish_path ="C:/Users/vicente\/ownloads\stockfish-windows-x86-64-avx2\stockfish.exe"

    engine = chess.engine.SimpleEngine.popen_uci(stockfish_path)
    engine.configure({"Skill Level": 20})

    board = chess.Board()

    # Escolha quem joga primeiro
    while True:
        choice = input("Escolha quem joga primeiro (1 para Oponente(Chess Challenge), 2 para IA(IA Clube da Nénem)): ")
        if choice == '1':
            break
        elif choice == '2':
            # A IA faz o primeiro movimento
            result = engine.play(board, chess.engine.Limit(time=2.0))
            print("Melhor jogada sugerida pela IA(Clube da Neném):", result.move)
            board.push(result.move)
            break
        else:
            print("Escolha inválida. Tente novamente.")

    while not board.is_game_over():
        display_board(board)

        # Lógica do jogador humano
        while True:
            try:
                move_uci = input("Digite seu movimento (notação UCI): ")
                move = chess.Move.from_uci(move_uci)
                if move in board.legal_moves:
                    board.push(move)
                    break
                else:
                    print("Movimento inválido. Tente novamente.")
            except ValueError:
                print("Entrada inválida. Use a notação UCI, por exemplo, 'e2e4'.")

        if board.is_checkmate():
            print("Checkmate! Você venceu!")
            break

        # Lógica da IA
        result = engine.play(board, chess.engine.Limit(time=2.0))
        print("Melhor jogada sugerida pela IA(Clube da Neném):", result.move)
        board.push(result.move)

        display_board(board)

        if board.is_checkmate():
            print("Checkmate! IA(Clube da Neném) venceu.")
            break

    print("Fim do jogo.")
    engine.quit()

if __name__ == "__main__":
    main()
