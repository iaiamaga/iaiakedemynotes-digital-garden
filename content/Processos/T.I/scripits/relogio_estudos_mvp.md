Acho excelente e **não é complicado** — dá pra ter funcionando em menos de 10 minutos! Vou te dar a visão completa e o script pronto.

---

## 📋 Visão do Projeto

O que você descreveu é um **MVP perfeito**: funcional, simples e útil. A lógica é direta:

```
START → inicia cronômetro (mostra tempo decorrido em tempo real)
PAUSE → pausa contagem, inicia contador de pausa, pode retomar
STOP → salva: data, tempo estudado, tempo pausado → arquivo DD-MM-AA
```

**Complexidade:** Baixa. Usamos apenas comandos nativos do Bash (`date`, `read`, variáveis, loops) — **zero dependências externas**.

---

## 🚀 Script Pronto: `relogio_estudos.sh`

Salve este arquivo em `~/relogio_estudos.sh`:

```bash
#!/bin/bash

# ============================================
# RELÓGIO DE ESTUDOS - MVP
# ============================================

ARQUIVO_LOG="tempo_estudos_$(date +%d-%m-%y).txt"

# --- Funções de tempo (segundos) ---
agora() { date +%s; }

# --- Formata HH:MM:SS ---
formatar_tempo() {
    local seg=$1
    printf "%02d:%02d:%02d" $((seg/3600)) $((seg%3600/60)) $((seg%60))
}

# --- Mostra contagem identada em tempo real ---
mostrar_contagem() {
    local estudo=$1
    local pausa=$2
    local status=$3
    
    # Limpa linha e volta pro início
    printf "\r\033[K"
    
    if [ "$status" = "estudando" ]; then
        printf "    📚 Estudando: %s    ⏸️  Pausa acumulada: %s" \
               "$(formatar_tempo $estudo)" "$(formatar_tempo $pausa)"
    else
        printf "    ⏸️  PAUSADO    📚 Estudado: %s    ⏸️  Pausa: %s" \
               "$(formatar_tempo $estudo)" "$(formatar_tempo $pausa)"
    fi
}

# ============================================
# LÓGICA PRINCIPAL
# ============================================

echo "=========================================="
echo "    ⏰ RELÓGIO DE ESTUDOS"
echo "=========================================="
echo ""
echo "Comandos:"
echo "  [ENTER] = Pausar / Retomar"
echo "  s + ENTER = Parar e Salvar"
echo ""
echo "Pressione ENTER para começar..."
read

# Inicializa variáveis
inicio=$(agora)
ultimo_inicio=$inicio
tempo_estudado=0
tempo_pausa=0
pausado=false
pausa_inicio=0

# Loop principal
while true; do
    if [ "$pausado" = false ]; then
        # Calcula tempo estudado
        agora_atual=$(agora)
        tempo_estudado=$((agora_atual - inicio - tempo_pausa))
        mostrar_contagem $tempo_estudado $tempo_pausa "estudando"
    else
        mostrar_contagem $tempo_estudado $tempo_pausa "pausado"
    fi
    
    # Verifica input não-bloqueante (timeout de 1s)
    if IFS= read -rs -t 1 tecla; then
        # Se digitou 's', para
        if [ "$tecla" = "s" ]; then
            break
        fi
        
        # Se ENTER (vazio), toggle pause
        if [ -z "$tecla" ]; then
            if [ "$pausado" = false ]; then
                # Pausar
                pausado=true
                pausa_inicio=$(agora)
            else
                # Retomar
                pausa_fim=$(agora)
                tempo_pausa=$((tempo_pausa + pausa_fim - pausa_inicio))
                pausado=false
            fi
        fi
    fi
done

# --- FINALIZAÇÃO ---
echo ""  # Nova linha após a contagem

# Se parou pausado, conta a última pausa
if [ "$pausado" = true ]; then
    pausa_fim=$(agora)
    tempo_pausa=$((tempo_pausa + pausa_fim - pausa_inicio))
fi

# Data e hora do fim
data_hora=$(date "+%d/%m/%Y %H:%M:%S")

# Salva no arquivo
{
    echo "=========================================="
    echo "  Registro de Estudo"
    echo "=========================================="
    echo "Data:        $data_hora"
    echo "Estudado:    $(formatar_tempo $tempo_estudado)"
    echo "Pausado:     $(formatar_tempo $tempo_pausa)"
    echo "------------------------------------------"
} >> "$ARQUIVO_LOG"

echo ""
echo "✅ Registro salvo em: $ARQUIVO_LOG"
echo "   📚 Tempo estudado: $(formatar_tempo $tempo_estudado)"
echo "   ⏸️  Tempo pausado:  $(formatar_tempo $tempo_pausa)"
echo ""
```

---

## ⚡ Como Usar (2 minutos)

```bash
# 1. Crie o arquivo
nano ~/relogio_estudos.sh
# Cole o script acima, salve (Ctrl+O, Enter, Ctrl+X)

# 2. Dê permissão de execução
chmod +x ~/relogio_estudos.sh

# 3. Execute
~/relogio_estudos.sh
```

**Controles durante a execução:**
- `ENTER` → Pausa / Retoma
- `s` + `ENTER` → Para e salva

---

## 📁 Exemplo de Arquivo Gerado

`tempo_estudos_30-06-26.txt`:
```
==========================================
  Registro de Estudo
==========================================
Data:        30/06/2026 14:35:22
Estudado:    00:45:30
Pausado:     00:10:15
------------------------------------------
```

---

## 📚 Referências Didáticas

| Conceito | O que usei | Referência Oficial |
|----------|-----------|-------------------|
| **Bash scripting** | Sintaxe completa | [GNU Bash Manual](https://www.gnu.org/software/bash/manual/bash.html) |
| **Comando `date`** | `date +%s` (epoch), formatação | [Coreutils Date](https://www.gnu.org/software/coreutils/manual/html_node/date-invocation.html) |
| **`read` com timeout** | `read -t 1` para input não-bloqueante | [Bash Builtins](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html) |
| **Escape ANSI** | `\r\033[K` para atualizar linha | [ANSI Escape Codes](https://en.wikipedia.org/wiki/ANSI_escape_code) |
| **Aritmética no Bash** | `$(( ))` para cálculos | [Shell Arithmetic](https://www.gnu.org/software/bash/manual/html_node/Shell-Arithmetic.html) |

---

## 💡 Próximos Passos (depois que testar)

Quando quiser evoluir, podemos adicionar:
- 🏷️ **Tags/categorias** (ex: "Matemática", "Programação")
- 📊 **Resumo semanal/mensal** (soma automática de horas)
- 🔔 **Notificações** (alerta a cada 25 min — técnica Pomodoro)
- 🖥️ **Interface gráfica** simples com `zenity` ou `yad`

---

**Testa aí e me diz se funcionou!** Se der algum erro, copia a mensagem que ajustamos na hora. 🚀

>[!NOTE]
>gerado com kimi modelo K2.6 Instant