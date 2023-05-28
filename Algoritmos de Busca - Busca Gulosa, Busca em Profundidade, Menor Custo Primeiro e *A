import numpy as np
import matplotlib.pyplot as plt

class AgentDFS:
    def __init__(self, ambiente) -> None:
        self.ambiente = ambiente
        self.percepcoes = ambiente.percepcoes_iniciais()
        self.F = [[self.percepcoes['posicao']]]
        self.C = []
        self.H = []
        self.C_H = []

    def act(self):

        # Executar ação
        # Coletar novas percepções

        plt.ion()
        while self.F:
            path = self.F.pop(0)
            
            action = {'mover_para':path[-1]}

            ################ VIS ###########
            plotar_caminho(self.ambiente,path,0.001)
            ################################

            self.percepcoes = self.ambiente.muda_estado(action) 

            if (self.percepcoes['posicao'] == self.percepcoes['saida']).all():
                ################ VIS ###########
                self.plotar_caminho(self.ambiente,path,3)
                ################################
                return path
            else:
                for vi in self.percepcoes['vizinhos']:
                    forma_ciclo = False
                    for no in path:
                        if (no == vi).all():
                            forma_ciclo = True

                    if not forma_ciclo:
                        self.F.append(path + [vi])
        
class AgentGreedy:

    def __init__(self, ambiente) -> None:
        self.ambiente = ambiente
        self.percepcoes = ambiente.percepcoes_iniciais()
        self.F = [[self.percepcoes['posicao']]]
        self.C = []
        self.H = [heuristica(self.percepcoes['posicao'],self.percepcoes['saida'])]
        self.C_H = []

    def act(self):

        # Executar ação
        # Coletar novas percepções

        plt.ion()
        while self.F:
            
            posicao_min_h_na_fronteira = np.argmin(self.H)
            
            path = self.F.pop(posicao_min_h_na_fronteira)
            self.H.pop(posicao_min_h_na_fronteira) 
            
            action = {'mover_para':path[-1]}

            ################ VIS ###########
            plotar_caminho(self.ambiente,path,0.001)
            ################################

            self.percepcoes = self.ambiente.muda_estado(action) 

            if (self.percepcoes['posicao'] == self.percepcoes['saida']).all():
                ################ VIS ###########
                self.plotar_caminho(self.ambiente,path,4)
                ################################
                return path
            else:
                for vi in self.percepcoes['vizinhos']:
                    forma_ciclo = False
                    for no in path:
                        if (no == vi).all():
                            forma_ciclo = True

                    if not forma_ciclo:
                        self.F.append(path + [vi])
                        self.H.append(heuristica(vi,self.percepcoes['saida']))

class MenorCustoPrimeiro:

    def __init__(self, ambiente) -> None:
        self.ambiente = ambiente
        self.percepcoes = ambiente.percepcoes_iniciais()
        self.F = [[self.percepcoes['posicao']]]
        self.C = []
        self.H = [custo_do_no(self.percepcoes['posicao'],[0,0])]
        self.C_H = []

    def act(self):

        # Executar ação
        # Coletar novas percepções

        plt.ion()
        while self.F:
            
            posicao_min_h_na_fronteira = np.argmin(self.H)
            
            path = self.F.pop(posicao_min_h_na_fronteira)
            self.H.pop(posicao_min_h_na_fronteira) 
            
            action = {'mover_para':path[-1]}

            ################ VIS ###########
            plotar_caminho(self.ambiente,path,0.001)
            ################################

            self.percepcoes = self.ambiente.muda_estado(action) 

            if (self.percepcoes['posicao'] == self.percepcoes['saida']).all():
                ################ VIS ###########
                self.plotar_caminho(self.ambiente,path,4)
                ################################
                return path
            else:
                for vi in self.percepcoes['vizinhos']:
                    forma_ciclo = False
                    for no in path:
                        if (no == vi).all():
                            forma_ciclo = True

                    if not forma_ciclo:
                        self.F.append(path + [vi])
                        self.H.append(custo_do_no(vi,self.percepcoes['posicao']))
                        self.F = sorted(self.F, key=lambda x: custo_do_no(x[-1], self.percepcoes['posicao']))
                        # A lista é colocada em ordem crescente. No caso, usando uma função lambida que pega como 
                        # referência o último valor da lista

class A:

    def __init__(self, ambiente) -> None:
        self.ambiente = ambiente
        self.percepcoes = ambiente.percepcoes_iniciais()
        self.F = [[self.percepcoes['posicao']]]
        self.C = []
        self.H = [custo_and_heuristica(self.percepcoes['posicao'],[0,0],self.percepcoes['saida'])]
        self.C_H = []

    def act(self):

        # Executar ação
        # Coletar novas percepções

        plt.ion()
        while self.F:
            
            posicao_min_h_na_fronteira = np.argmin(self.H)
            
            path = self.F.pop(posicao_min_h_na_fronteira)
            self.H.pop(posicao_min_h_na_fronteira) 
            
            action = {'mover_para':path[-1]}

            ################ VIS ###########
            plotar_caminho(self.ambiente,path,0.001)
            ################################

            self.percepcoes = self.ambiente.muda_estado(action) 

            if (self.percepcoes['posicao'] == self.percepcoes['saida']).all():
                ################ VIS ###########
                self.plotar_caminho(self.ambiente,path,4)
                ################################
                return path
            else:
                for vi in self.percepcoes['vizinhos']:
                    forma_ciclo = False
                    for no in path:
                        if (no == vi).all():
                            forma_ciclo = True

                    if not forma_ciclo:
                        self.F.append(path + [vi])
                        self.H.append(custo_do_no(vi,self.percepcoes['posicao']))
                        self.F = sorted(self.F, key=lambda x: custo_and_heuristica(self.percepcoes['posicao'],x[-1],self.percepcoes['saida']))
                        # A lista é colocada em ordem crescente. No caso, usando uma função lambida que pega como 
                        # referência o último valor da lista

def heuristica(no, objetivo):
  manhattan_distance = np.sum(np.abs(no-objetivo))
  return manhattan_distance

def custo_do_no(no, entrada):
  no_distance = np.sum(np.abs(no-entrada))
  return no_distance

def custo_and_heuristica(no, entrada, objetivo):
  no_heuristic_distance = np.sum(np.abs(no-entrada)) + np.sum(np.abs(no-objetivo))
  return no_heuristic_distance

def plotar_caminho(ambiente, caminho, tempo):
  plt.axes().invert_yaxis()
  plt.pcolormesh(ambiente.map)
  for i in range(len(caminho)-1):
      plt.plot([caminho[i][1]+0.5,caminho[i+1][1]+0.5],[caminho[i][0]+0.5,caminho[i+1][0]+0.5],'-rs')
  plt.draw()
  plt.pause(tempo)
  plt.clf()
