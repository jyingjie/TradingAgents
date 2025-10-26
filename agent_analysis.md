# Agent Analysis

This document provides an analysis of the agent architecture in the TradingAgents project.

## 1. Agent Functionality

- Each agent is a specialized AI assistant responsible for a specific area of analysis (e.g., fundamentals, market sentiment, news).
- Agents use a combination of a language model (LLM) and a set of tools to perform their analysis.
- The goal of each agent is to produce a report summarizing its findings.
- The reports from all agents are then used in a series of debates and discussions to arrive at a final trading decision (BUY/SELL/HOLD).

## 2. Agent Implementation

- **Agent Nodes:** Each agent is implemented as a "node" in a larger graph structure. These nodes are functions that take the current state of the system as input and return an updated state with their report.
- **`create_*_analyst` functions:** These are factory functions that create the agent nodes. They configure the agent with a specific LLM, a set of tools, and a system message that defines its role and instructions.
- **`langgraph`:** The project uses the `langgraph` library to build and execute the graph of agents. This library allows for a flexible and modular architecture where agents can be easily added, removed, or reconfigured.
- **State Management:** The graph operates as a state machine. The state is a dictionary that is passed from node to node, accumulating information and reports as the graph is executed.
- **Tools:** Agents are given access to a set of tools that allow them to fetch data from various sources (e.g., financial data from yfinance or Alpha Vantage, news from Google News).
- **LLMs:** The system uses two LLMs, a "deep thinking" model and a "quick thinking" model, which can be configured to use different models from different providers (OpenAI, Anthropic, Google).

## 3. End-to-End Workflow

1. **Initialization:** The `TradingAgentsGraph` is initialized with a configuration that specifies which analysts to use, which LLM models to use, and which data vendors to use.
2. **Propagation:** The `propagate` method is called with a company ticker and a date. This kicks off the execution of the graph.
3. **Analysis:** The analyst agents run in parallel, each producing a report on their area of expertise.
4. **Debate:** The reports are then fed into a series of debate and discussion nodes, where different agents (e.g., "bull" vs. "bear" researchers, risk managers) argue their case.
5. **Decision:** Finally, a "trader" agent makes a final investment decision based on the outcome of the debates.
6. **Reflection (Optional):** The system has a `reflect_and_remember` method that allows the agents to learn from their mistakes by reflecting on the outcome of their past decisions.
